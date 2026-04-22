# wiki-update

Update the **UniPack GitHub wiki** to reflect the current branch.

## Scope

- UniPack may use a separate wiki clone; maintainers often set `UNIPACK_WIKI_DIR` (see `dev/scripts/release.sh`). If you only have this repo, ask whether a wiki path is available or whether to draft sections as markdown snippets for manual paste.
- Keep pages **user-friendly, clear, and concise**—installation, supported package managers, keyboard map, troubleshooting (missing `yay`/`paru`, permissions, etc.).

## Editing rules

- Prefer improving existing pages over spawning many tiny ones.
- Add a dedicated page only when the topic is large (e.g. a full backend guide); otherwise fold into the right existing section.
- Stay consistent with `README.md` and `SPEC.md` on facts (keys, flags, supported tools). If the branch changes product behavior, the wiki must match.

If no wiki checkout is in workspace, produce the proposed markdown in chat (or a path the user names) and state what should go where.
