# git-staged-msg

Create or refresh a **commit message** for **currently staged** files. Write to **`dev/COMMIT/`** (gitignored except the command workflow—use e.g. `dev/COMMIT/message.txt` for full body and `dev/COMMIT/short.txt` for the subject line, matching whatever files already exist in that folder).

## Conventional commits

Infer the lead type from the change set:

- `fix:`, `feat:`, `change:`, `perf:`, `test:`, `chore:`, `refactor:`, `docs:`, `style:`, `build:`, `ci:`, `revert:`

## Format

```
<type>: <short summary>

- <type>: bullet, short and specific
- <type>: bullet
```

No extra prose outside the commit text. Bullets should map to staged logical units when there are several.
