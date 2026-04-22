# Security Policy

## Supported Versions

Security-sensitive fixes are applied on the **active development branch** and released for the **current minor series** (see `Cargo.toml` and [CHANGELOG.md](CHANGELOG.md)).

| Version   | Supported          |
| --------- | ------------------ |
| 0.1.x     | :white_check_mark: |
| Older     | :x:                |

## Reporting a Vulnerability

If you believe you have found a security issue in **UniPack** (this repository’s `unipack` binary and library code), please report it responsibly.

**Preferred:** use [GitHub private vulnerability reporting](https://github.com/aliabdoxd14-sudo/unipack/security/advisories/new) for this repository, if it is enabled for your account.

**Alternative:** email **firstpick1992@proton.me** with the subject **`[UniPack Security]`**. If email is not possible, open a GitHub issue with minimal public detail and include **“Security”** in the title; maintainers will triage and may follow up privately.

Please include, when possible:

- UniPack version (e.g. from `unipack --help` / `Cargo.toml`) and how you installed it (source, `cargo install`, Arch `PKGBUILD-git`, etc.)
- OS and environment (distribution, shell, relevant privilege model if it involves `sudo`)
- Reproduction steps and expected vs. actual behavior
- Impact assessment and a proof-of-concept if available
- Relevant logs or screenshots (redact secrets)

**What to expect**

- Acknowledgement within a few business days when contact details are valid
- Status updates while the issue is open
- Coordinated disclosure when applicable (timing and credit, or anonymity if you prefer)

**Out of scope**

- Vulnerabilities in third-party package managers (`npm`, `pip`, `pacman`, Flatpak, Snap, Homebrew, etc.), their registries, or your distribution’s packages
- Non-security bugs (use [regular issues](https://github.com/aliabdoxd14-sudo/unipack/issues))

Thank you for helping keep UniPack and its users safe.
