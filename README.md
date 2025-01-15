# Git Commit Info Action

This GitHub Action extracts commit information like sha, sha_short, username, and URL from commits.

## Inputs

- `ACCESS_TOKEN` (required): A GitHub personal access token with repository access.

## Example Usage

```yaml
name: Example Workflow

on: 
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  commit_info:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v2

    - name: Get Commit Info
      uses: username/git-commit-info@v1
      with:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
