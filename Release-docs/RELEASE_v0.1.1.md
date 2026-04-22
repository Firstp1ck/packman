# UniPack v0.1.1

## Focus

UniPack is now an **update tool**, not an installer. Use your native package manager to install new packages; use UniPack to see and apply updates across all of them from one place. Remove is still supported for cleaning up what you already have.

## Highlights

- **Security fix — shell injection closed:** privileged actions (upgrade and remove) no longer pass package names through `sh -c`. Names are handed to each package manager as a direct argument, so shell metacharacters can never be interpreted. This removes a real injection path that previously existed when typing into the search box.
- **Install feature removed:** the `i` keybinding and in-app install flow are gone. UniPack now focuses entirely on upgrades and removals. Install new packages with your native tool (e.g. `sudo pacman -S …`, `sudo apt install …`, `pip install …`).
- **Safer privileged actions:** UniPack checks sudo readiness before privileged remove and upgrade operations, with clearer one-time guidance for apt/pacman/AUR/rpm/snap flows.
- **Better upgrade feedback:** single-package upgrades show a clearer progress strip with elapsed-time feedback.
- **Faster bulk update workflow:** the all-upgradables overlay supports search filtering and clearer empty-state handling when no matches are found.
- **Improved platform behavior:** distro detection and key handling were refined for more consistent behavior across environments.

## Breaking changes

- The `i` key no longer installs packages. If you scripted or muscle-memoried the `/`-then-`i` flow, switch to your package manager's CLI for installs.
- The `i` entry is removed from the footer hint and `unipack --help` output.

## Other changes

- Project naming and repository references were aligned around **UniPack** for consistency.
- Documentation (`README.md`, `SPEC.md`) and helper scripts were refreshed to match the update-focused scope.
