# Git Commit Info GitHub Action

This GitHub Action extracts commit information, such as the full SHA, short SHA, username of the commit author, commit message, and commit URL. It can be useful for workflows that need detailed commit metadata.

This action is designed to work across platforms (Linux, macOS, and Windows). It uses PowerShell (`pwsh`) to extract and process commit information.

## Inputs

The action requires the following input:

- **`ACCESS_TOKEN`**: GitHub personal access token (with repository access). This is required for making authenticated requests to the GitHub API.

## Outputs

The action provides the following outputs:

- **`username`**: Username of the commit author.
- **`sha`**: Full commit SHA.
- **`sha_short`**: Shortened commit SHA (first 7 characters).
- **`url`**: URL to the commit on GitHub.
- **`commit_message`**: The commit message associated with the commit.

## How It Works

The action checks the type of event that triggered the workflow:

- **For Pull Requests**: It retrieves the commit details from the head of the pull request.
- **For Push Events**: It uses the current commit (`github.sha`) to extract commit information.

The action then fetches additional information, including the username of the commit author, the commit message, and the commit URL, using GitHub's REST API.

## Example Usage

To use this GitHub Action in your workflow, follow the example below:

```yaml
name: 'Extract Commit Info'

on: 
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  extract_commit_info:
    runs-on: ubuntu-latest  # Change to 'windows-latest' or 'macos-latest' if needed
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
        
      - name: Extract Commit Info
        id: extract_commit_info
        uses: zxc30314/git-commit-info-action@v1
        with:
          ACCESS_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Show Commit Information
        run: |
          echo "Commit SHA: ${{ steps.extract_commit_info.outputs.sha }}"
          echo "Commit Short SHA: ${{ steps.extract_commit_info.outputs.sha_short }}"
          echo "Commit URL: ${{ steps.extract_commit_info.outputs.url }}"
          echo "Commit Author: ${{ steps.extract_commit_info.outputs.username }}"
          echo "Commit Message: ${{ steps.extract_commit_info.outputs.commit_message }}"
