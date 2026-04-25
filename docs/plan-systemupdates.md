# UniPack Plan: Verified System-Update Strategy and Full-Selection Guards

## Goal

Add a safe and deterministic "system update" path that uses package-manager-native full-upgrade
commands only when selection context is provably complete and fresh. Otherwise, UniPack keeps the
current per-package upgrade behavior.

Scope:

- overlay selection and upgrade trigger path (`src/overlay.rs`, `src/run_loop.rs`, `src/ui.rs`)
- upgradable-row source (`src/all_upgradables.rs`)
- backend command execution (`src/pkg_manager/commands.rs`, `src/pkg_manager/mod.rs`)

---

## Verified current-state audit (code-accurate)

### 1) Bulk upgrade path today

- `a` opens the all-upgradables overlay from cached per-backend lists (`open_all_upgradables_overlay` in `src/run_loop.rs`).
- `u` in overlay upgrades selected rows (`handle_overlay_selection` -> `upgrade_all_upgradables_selection` in `src/overlay.rs`).
- A worker thread executes upgrades sequentially via `pm.upgrade_package(&op_arg)` for each selected row (`spawn_multi_upgrade_worker` in `src/overlay.rs`).
- No dedicated full-system command path exists.

### 2) Selection model today

- Overlay rows are `UpgradableRow` values (`src/all_upgradables.rs`).
- Selection is index-based (`BTreeSet<usize>`) in `AllUpgradablesOverlay` (`src/model.rs`).
- `select all` means all overlay rows, regardless of backend (`overlay_select_all_rows` in `src/overlay.rs`).
- No freshness revalidation occurs immediately before executing upgrades.

### 3) Supported backend reality (important correction)

Current detected backend names are:

- `pip`, `npm`, `pnpm`, `bun`, `cargo`, `brew`, `apt`, `pacman`, `aur`, `rpm`, `flatpak`, `snap`

Notes:

- There is no `dnf`, `zypper`, `xbps-install`, `apk`, or `eopkg` backend in current detection.
- AUR is represented as backend name `aur`; actual binary is `yay` or `paru` in `PackageManager.command`.

---

## Refined command policy

### Critical warning: avoid partial system package upgrades

- For system package managers (`pacman`, `aur`, `apt`, and similar distro-level backends), upgrading a
  single package can fail or leave the system in an inconsistent state when repository metadata and
  dependency versions are out of sync.
- Recommended default is a complete system update for that backend (`-Syu`, `apt update && apt upgrade`,
  etc.), not ad-hoc single-package upgrades.
- UniPack should surface this clearly in UI/status messaging whenever users attempt single-package
  upgrades on system-level backends.

### 1) Full-update commands (only for currently supported backends)

- `pacman`: `sudo pacman -Syu`
- `aur` (`yay`/`paru`): `<aur_binary> -Syu`
- `apt`: `sudo apt update` then `sudo apt upgrade -y`
- `flatpak`: `flatpak update -y`
- `snap`: `sudo snap refresh`

Not planned as full-system command in v1:

- `pip`, `npm`, `pnpm`, `bun`, `cargo`, `brew`, `rpm`

Reason: these are currently package-oriented in UniPack and do not map cleanly to a consistent,
safe "system-wide full-upgrade" semantic in this app.

### 2) Fallback rule

- If full-update is not explicitly allowed, execute existing per-package upgrades.
- Never silently switch to full-update.
- If a full-update action is requested but denied, show exact reason and recommended next action.

---

## Safety invariant (revalidated at execution time)

A backend may run full-update only if all checks pass:

1. **Fresh overlay snapshot**
   - Rebuild current upgradable rows from cache with `collect_upgradables_from_cached_lists(...)`.
   - Require rebuilt rows to match overlay rows (same rows/order) before executing.
2. **Non-empty target**
   - Target backend has at least one currently upgradeable row.
3. **True backend-complete selection**
   - Selected rows include all currently upgradeable rows for that backend.
4. **No unknown backend policy**
   - Backend is in the explicit full-update allowlist above.

If any check fails:

- deny full-update for that backend
- keep package-level upgrades for selected rows
- report reason (`stale_overlay`, `partial_selection`, `unsupported_backend`, `empty_target`)

---

## Architecture changes

### 1) Add a shared resolver module

Add `src/pkg_manager/system_update_policy.rs`:

- resolves execution plan from overlay rows + selected indices + current PM state
- maps backend to full-update command spec
- returns structured decision per backend:
  - `FullSystemUpdate { pm_index, command_spec }`
  - `PackageLevelUpgrade { tasks }`
  - denial reason metadata

Also export from `src/pkg_manager/mod.rs` as needed.

### 2) Add overlay freshness metadata

Extend `AllUpgradablesOverlay` (`src/model.rs`) with:

- `opened_row_count: usize`
- `opened_backend_counts: BTreeMap<usize, usize>` (rows per `pm_index` at open)

These are cheap hints for UI/status; authoritative guard remains runtime re-collection comparison.

### 3) Single execution entrypoint

Route all overlay upgrade triggers through one resolver-driven function in `src/overlay.rs`:

- current `u` behavior
- future "instant system update" action

No ad-hoc command decisions in key handlers.

### 4) UI action surface

Phase 1 (minimal UX change):

- keep `u` as main action
- when selection qualifies backend(s) for full-update, show hint in footer/status (for example: `u = upgrade selected (full-update where eligible)`).

Phase 2 (optional explicit action):

- add dedicated key for forced full-update attempt (example: `U`), which still runs the same resolver and safety checks.

---

## Implementation phases

### Phase 1: Resolver + command mapping

1. Add `system_update_policy` module.
2. Implement backend allowlist and command-spec generation.
3. Add structured deny reasons.

Acceptance:

- unit tests for command mapping and allow/deny decisions.

### Phase 2: Freshness and selection guards

1. Add overlay metadata fields in `src/model.rs`.
2. At execute time, rebuild current rows and validate freshness.
3. Compute per-backend completeness from selected indices.

Acceptance:

- stale or partial contexts cannot run full-update.

### Phase 3: Overlay integration

1. Refactor overlay upgrade execution to use resolver output.
2. Keep sequential worker for package-level tasks unchanged.
3. Add message text describing selected mode per backend.

Acceptance:

- deterministic behavior: same input state -> same execution plan.

### Phase 4: Optional explicit instant action

1. Add explicit keybinding and footer hint.
2. Reuse same resolver and denial reasons.
3. Keep existing `u` backward-compatible.

Acceptance:

- explicit action cannot bypass guards.

### Phase 5: Hardening

1. Missing-binary diagnostics (`aur` helper changed/unavailable, etc.).
2. Clear per-backend result messages for mixed plans.
3. Regression tests around stale overlay + mixed backend selection.

---

## Test plan

### Unit tests (`system_update_policy`)

- full selection + fresh rows + supported backend => `FullSystemUpdate`
- partial selection => `PackageLevelUpgrade` with deny reason
- stale rows => deny full-update
- unsupported backend selected fully => deny full-update
- mapping correctness (`pacman`, `aur`, `apt`, `flatpak`, `snap`)
- mixed backend selection yields mixed execution plan

### Integration-style tests (overlay behavior)

- overlay open -> select all -> execute:
  - full-update selected for eligible backends
  - package-level for ineligible backends
- stale overlay (cached list changed before execute):
  - full-update denied
  - package-level path preserved
- partial selection:
  - no full-update triggered

### Regression tests

- AUR backend uses active helper command (`yay` or `paru`) from `PackageManager.command`
- "all rows selected" still denied if overlay became stale
- fallback path remains equivalent to current package-by-package behavior

---

## Risks and mitigations

- **Risk:** accidental behavior drift in existing upgrades.
  - **Mitigation:** keep current per-package worker path as default/fallback.
- **Risk:** stale overlay causing incorrect full-update.
  - **Mitigation:** execute-time re-collection and equality check against overlay rows.
- **Risk:** command mismatch across distros/tools.
  - **Mitigation:** limit full-update to explicit supported backends and test mapping table.

---

## Deliverables

1. `src/pkg_manager/system_update_policy.rs` with unit tests.
2. Overlay metadata updates in `src/model.rs` + open-path setup in `src/run_loop.rs`.
3. Resolver-driven execution wiring in `src/overlay.rs`.
4. Footer/status messaging updates in `src/ui.rs`.
5. Integration/regression tests for stale/partial/mixed selection behavior.

---

## Final policy statement

UniPack may run a backend-native full-update command only when execute-time validation proves
the overlay is fresh and the selected rows fully cover that backend’s current upgradeable set;
otherwise UniPack must use package-level upgrades and explain why full-update was not applied.

