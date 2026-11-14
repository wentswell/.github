# AI review focus: .github

The AI review (`.github/workflows/ai-review.yml`) reads this file. It lists where this repository breaks. Keep it short: one line per fault class.

Look hardest at these, which are where this repository breaks:
- A change to `codex-review.yml` that breaks a caller: a renamed input or
  secret, a job that now needs a permission the callers do not grant.
- A job-level `permissions` block that drops a scope a step needs (a
  job-level block replaces the default).
- The `post` script mapping a finding to a line that is not on the new side
  of the diff, or losing a finding when GitHub refuses the review.

Check these for security too:
- The job that runs the model gaining a write scope or a token in its
  environment.
- Text from the event (a comment, a title, a branch name) interpolated into
  a `run:` script instead of passed through `env`.
- A path that lets a fork's code reach the job that holds the API key.
