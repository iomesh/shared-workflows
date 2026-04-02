# Shared Workflows

Reusable GitHub Actions workflows for iomesh.

## Claude Code (Bedrock)

AI-powered PR assistant using Claude via AWS Bedrock.

### Usage

Add `.github/workflows/claude.yml` to your repo:

```yaml
name: Claude Code
on:
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

### Custom parameters

```yaml
jobs:
  claude:
    uses: iomesh/shared-workflows/.github/workflows/claude-bedrock.yml@main
    with:
      model: "ap-southeast-1.anthropic.claude-opus-4-6-v1"
      max_turns: "20"
    secrets:
      AWS_ROLE_TO_ASSUME: ${{ secrets.AWS_ROLE_TO_ASSUME }}
      APP_ID: ${{ secrets.APP_ID }}
      APP_PRIVATE_KEY: ${{ secrets.APP_PRIVATE_KEY }}
```

### Required organization secrets

| Secret | Value |
|--------|-------|
| `AWS_ROLE_TO_ASSUME` | `arn:aws:iam::457615671886:role/GitHubActionsClaudeCode` |
| `APP_ID` | GitHub App ID |
| `APP_PRIVATE_KEY` | GitHub App private key (.pem) |
