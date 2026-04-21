# release-new

Create a new release note at `Release-docs/RELEASE_v{version}.md` for the given version.

## How to gather changes

- Determine the previous release: latest `Release-docs/RELEASE_v*.md` or the previous git tag, whichever the user cares about for this run.
- Review `git log` / diff since that point; group into user-visible themes (behavior, backends, UX, fixes).

## Tone and shape

- Match the style of existing notes (e.g. `Release-docs/RELEASE_v0.1.0.md`): short sections, plain language, **what users gain** over internal refactors.
- Call out breaking behavior, new/removed backends, and keybinding or flag changes explicitly.
- Point to `README.md` for install, supported managers, and key reference where it avoids duplication.

## Maintainer pointer

- Optional automation and AUR/wiki steps live in `dev/scripts/release.sh`; do not paste long maintainer-only procedure into the release doc unless the user asks.

Keep the file user-friendly, concise, and easy to skim.
