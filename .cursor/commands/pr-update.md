# pr-update

Compare the current branch against **`main`** (`git log main..HEAD` and `git diff --name-status main...HEAD`). If the default branch is not `main`, use the repo’s real default after checking `git symbolic-ref refs/remotes/origin/HEAD` or `git remote show origin`.

Update the matching file in **`dev/PR/`** for the active branch (same layout as `/pr`).

## Merge discipline

Integrate missing updates into existing sections (**Summary**, **Related issues**, **How to test**, **Files changed**, checklist) so the PR reads as one coherent document. Do **not** add a standalone “additional updates” append-only section unless the user asked.

When updating:

- Keep wording short, specific, and reviewer-focused.
- Only describe what differs from the base branch (final branch state).
- Keep valid bullets but merge/dedupe for clarity.
- Remove stale statements (e.g. “remaining work” that is done).
- **How to test** must match current behavior: `cargo fmt`, `cargo clippy ... -D warnings`, `cargo test`, and manual TUI steps when UX or backends changed.
