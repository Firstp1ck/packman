# announce-version

Create a short announcement for the **given UniPack version** (release day blurb).

- User-friendly, **max ~800 characters** for the postable summary.
- Save to **`dev/ANNOUNCEMENTS/version_announcement_content.md`** (create `dev/ANNOUNCEMENTS/` if missing).

## Format

```markdown
## What's New

- Feature 1
- Feature 2
- Fix 1
- Improvement 1
- Chore 1
- Refactor 1
```

Use bullets only for real items; omit empty categories. Align facts with `Release-docs/RELEASE_v{version}.md` when that file exists.
