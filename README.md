# .github

Shared GitHub Actions workflows for the `wentswell` organization.

## Workflows

### Codex review

`.github/workflows/codex-review.yml` is a reusable workflow. Codex reads a pull
request and posts one pull request review. Each finding is an inline comment
on its line, so each finding is a review thread you can resolve.

Each repository calls it from `.github/workflows/ai-review.yml`. The
canonical copy of that caller is `workflow-templates/ai-review.yml`, and every
repository holds it byte for byte. It is also offered as an organization
workflow template under **Actions > New workflow**. The caller holds the
triggers, the concurrency group, and the permissions:

```yaml
jobs:
  review:
    uses: wentswell/.github/.github/workflows/codex-review.yml@main
    permissions:
      contents: read
      issues: write
      pull-requests: write
    secrets:
      OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

The caller grants the most any job may have. Each job in the shared workflow
then narrows it. The job that runs the model gets read access only.

A review runs when a pull request opens or becomes ready for review. A push
starts no review. To run it again, comment `/ai-review` on the pull request.
Only people with write access can do this.

Each repository can add `.github/ai-review-focus.md`. It lists the faults that
repository is most likely to have. Codex reads it before the diff.

Changes to the shared workflow reach every repository at once, because the
callers use `@main`. This repository reviews its own pull requests with the
local copy, so a change to the shared workflow is tested on its own pull
request before it reaches `main`.

The `model` and `effort` inputs default to `gpt-5.6-sol` and `medium`.
