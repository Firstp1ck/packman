# pr

Create a PR description for the current UniPack change. Save it as `dev/PR/<branch-name>.md` (that directory is gitignored for local drafts).

This repository does not ship a `PULL_REQUEST_TEMPLATE.md` yet—use the following sections so they line up with `/pr-update`:

## Sections

1. **Title suggestion** — one line, imperative mood (e.g. "Fix flatpak list refresh when search is active").
2. **Summary** — what changed and why (maintainer-focused, not a dump of commits).
3. **Related issues** — `Fixes #…` / `See #…` if applicable; omit if none.
4. **How to test** — include at least:
   - `cargo fmt --all -- --check`
   - `cargo clippy --all-targets --all-features -- -D warnings`
   - `cargo test`
   - Short **manual TUI** steps if behavior or keys changed (which backend, which keys).
5. **Files / areas touched** — high-signal list (modules or themes), not every file unless small PR.
6. **Checklist** — fmt, clippy, tests, manual smoke if relevant; security-sensitive paths in `src/pkg_manager.rs` get an extra glance.

Keep wording short and specific. Follow `AGENTS.md` for Rust/TUI expectations.
