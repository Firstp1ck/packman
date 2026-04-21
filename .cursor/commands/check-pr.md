# check-pr

For the PR under review: inspect **recent commits by the given author** (provide Git name, email, or GitHub handle as the user directs).

## Output

- Walk through what each commit changed and how it fits together.
- If you spot **critical logic issues** (especially shell invocation, quoting, timeouts, or backend detection in `src/pkg_manager.rs`), explain them and suggest concrete fixes or tests.

Use `git log` scoped to the PR branch range vs base (usually `main`) as appropriate.
