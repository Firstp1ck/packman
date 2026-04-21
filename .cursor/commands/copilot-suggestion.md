# copilot-suggestion

Evaluate the **Copilot (or similar) suggestion** the user points to for this **Rust** codebase.

## PackMan-specific checks

- **`unwrap` / `expect`:** new code should prefer `Result`; `unwrap_used` is denied for library-style paths—reject suggestions that add casual unwraps on user or command output data.
- **`src/pkg_manager.rs`:** shell strings and `Command` construction are security-sensitive; reject weak quoting, unbounded user input in `sh -c`, or missing timeouts where async helpers expect them.
- **Lint policy:** suggestion must survive `cargo clippy --all-targets --all-features -- -D warnings` and `cargo fmt --all`.

If valid, implement it minimally. If invalid, explain why and skip—no drive-by refactors.
