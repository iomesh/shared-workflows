# Shared Workflows

Reusable GitHub Actions workflows for iomesh.

## Claude Code (Bedrock)

AI-powered code review and PR assistant using Claude via AWS Bedrock.

### Features

- Auto-reviews PRs on open/update
- Responds to `@claude` in PR comments and issues
- Uses AWS Bedrock via OIDC (no static keys)

### Usage

Add `.github/workflows/claude.yml` to your repo:

```yaml
name: Claude Code
on:
  pull_request:
    types: [opened, synchronize]
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]

jobs:
  claude:
    uses: iomesh/shared-workflows/.github/workflows/claude-bedrock.yml@main
    secrets:
      AWS_ROLE_TO_ASSUME: ${{ secrets.AWS_ROLE_TO_ASSUME }}
      APP_ID: ${{ secrets.APP_ID }}
      APP_PRIVATE_KEY: ${{ secrets.APP_PRIVATE_KEY }}
```

If replacing an existing `claude-review.yml`, delete the old file to avoid duplicate triggers.

### Custom parameters

```yaml
jobs:
  claude:
    uses: iomesh/shared-workflows/.github/workflows/claude-bedrock.yml@main
    with:
      model: "ap-southeast-1.anthropic.claude-opus-4-6-v1"
      max_turns: "30"
      prompt: "Custom review instructions here"
    secrets:
      AWS_ROLE_TO_ASSUME: ${{ secrets.AWS_ROLE_TO_ASSUME }}
      APP_ID: ${{ secrets.APP_ID }}
      APP_PRIVATE_KEY: ${{ secrets.APP_PRIVATE_KEY }}
```

### Available inputs

| Input | Default | Description |
|-------|---------|-------------|
| `model` | `ap-southeast-1.anthropic.claude-sonnet-4-6-v1` | Bedrock model ID |
| `max_turns` | `25` | Max conversation turns |
| `aws_region` | `ap-southeast-1` | AWS region |
| `prompt` | `"Review this PR..."` | Prompt for auto-review |
| `additional_permissions` | `Bash(*),Edit(*),Write(*)` | Tool permissions |
| `track_progress` | `true` | Show progress tracking |

### Required organization secrets

| Secret | Value |
|--------|-------|
| `AWS_ROLE_TO_ASSUME` | `arn:aws:iam::457615671886:role/GitHubActionsClaudeCode` |
| `APP_ID` | GitHub App ID |
| `APP_PRIVATE_KEY` | GitHub App private key (.pem) |
