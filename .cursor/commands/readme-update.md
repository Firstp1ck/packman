# readme-update

Update **PackMan** `README.md` for the changes on the current branch.

This command is available in chat as `/readme-update`.

## Rules

- User-friendly, short, and scannable. PackMan is a **Rust TUI** unifying multiple package managers—keep that framing accurate.
- Do **not** add new top-level sections for tiny edits; fold updates into existing structure (features table, shortcuts, install, requirements).
- Align supported managers, keys, and install steps with `SPEC.md` / real code (`src/lib.rs`, `src/pkg_manager.rs`) when behavior changed.

Save the result in `README.md` at the repo root.
