# UniPack v0.1.4

## Highlights

- **Fuzzy search mode in both package views.** While search is active, press `Ctrl+f` to toggle normal substring matching and fuzzy subsequence matching in the main list and the all-upgradables overlay.
- **Backend-aware bulk upgrade behavior.** In the all-upgradables view, UniPack now distinguishes package-level upgrades from backend-native full-system updates, then executes the safest matching strategy for each selected backend.
- **Safer system update actions.** Full-system updates now use a verified support policy and explicit confirmation flow so unsupported or stale selections are handled more predictably.
- **Quick full-system update shortcut.** On supported package-manager tabs, `Ctrl+u` opens a confirmation prompt and runs the backend-native full-system update from the main view.

## Changed behavior

- **New search toggle:** `Ctrl+f` switches normal ↔ fuzzy matching while search mode is enabled.
- **New system-update shortcut:** `Ctrl+u` is now reserved for full-system update on the active package manager (when supported) instead of page-up movement.
- **Footer/help hints updated:** key hints now surface system-update availability and the revised paging behavior.

## Other changes

- Internal planning docs were expanded for potential future backend coverage (conda, dnf, gem, pipx, and zypper).
- Release automation and packaging metadata were updated during this cycle.

For install options, supported package managers, and the key reference, see [README](../README.md). Maintainer automation steps (tagging/AUR/wiki helpers) are available in `dev/scripts/release.sh`.
