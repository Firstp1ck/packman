# translate

UniPack is **English-only** today: there are no locale JSON files or i18n pipeline like `check_translation_keys.py`.

Use this command as a **user-visible copy audit** after UI or CLI changes:

1. Scan **user-facing strings** in `src/` (TUI labels, footers, errors, dialogs) and `unipack --help` text paths in `src/main.rs` / `src/lib.rs`.
2. Check consistency with **keyboard hints** (`render_footer()` and help output must agree per `AGENTS.md`).
3. Flag unclear, duplicated, or misleading wording; propose concise replacements.
4. If a string encodes a **backend name or command**, verify it matches real detection in `src/pkg_manager.rs`.

Do **not** invent locale files or translation keys unless the project adds an i18n system first.
