<!-- Use this file to provide workspace-specific custom instructions to Copilot. For more details, visit https://code.visualstudio.com/docs/copilot/copilot-customization#_use-a-githubcopilotinstructionsmd-file -->

# Secret Scanning Alert Checker

This workspace contains a GitHub Actions workflow that checks if secrets from GitHub secret scanning alerts are still present in a specified branch.

## Key Components

- **Workflow File**: `.github/workflows/check-secrets-in-branch.yml` - Main GitHub Actions workflow
- **GitHub API Integration**: Uses GitHub CLI (`gh`) to interact with secret scanning alerts
- **Branch Comparison**: Compares alert locations with current file content on a specified branch

## Workflow Features

1. **Alert Retrieval**: Gets all secret scanning alerts for a repository
2. **Location Processing**: Processes each alert location to extract file paths and line/column information
3. **Content Comparison**: Checks if the secret still exists at the specified location in the target branch
4. **Reporting**: Generates comprehensive summaries and artifacts
5. **Failure Handling**: Fails the workflow if secrets are found

## Usage Patterns

- Manual trigger with branch specification
- Scheduled runs for regular checking
- Integration with other security workflows

When working with this codebase, focus on GitHub Actions syntax, secret scanning API endpoints, and bash scripting for file content processing.
