# Secret Scanning Alert Checker

A GitHub Actions workflow that checks if secrets from GitHub secret scanning alerts are still present in a specified branch.

## Overview

This workflow retrieves secret scanning alerts from a repository and verifies whether the secrets are still present in the current version of files on a specified branch. It's useful for:

- Verifying that secrets have been properly removed from a branch
- Monitoring secret exposure across different branches
- Automating security checks in your CI/CD pipeline

## Features

- ✅ **Automated Alert Retrieval**: Fetches all secret scanning alerts from the repository
- ✅ **Branch-Specific Checking**: Verifies secrets against any specified branch
- ✅ **Detailed Reporting**: Generates comprehensive summaries with file locations and line numbers
- ✅ **Artifact Generation**: Saves results as JSON artifacts for further processing
- ✅ **Flexible Triggers**: Supports manual dispatch and scheduled runs
- ✅ **Failure Handling**: Fails the workflow if secrets are found

## Usage

### Manual Trigger

1. Go to the Actions tab in your repository
2. Select "Check Secret Scanning Alerts in Branch"
3. Click "Run workflow"
4. Specify:
   - **Branch**: The branch to check (default: main)
   - **Repository**: The repository to check (default: current repo)

### Scheduled Runs

The workflow automatically runs daily at 6 AM UTC to check for secrets in the default branch.

### Workflow Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `branch` | Branch to check for secrets | Yes | `main` |
| `repository` | Repository to check (owner/repo format) | No | Current repository |

## Permissions Required

The workflow requires the following permissions:
- `contents: read` - To read repository files
- `security-events: read` - To access secret scanning alerts

## Output

The workflow generates:

1. **Step Summary**: A detailed report showing:
   - Total locations checked
   - Secrets still present
   - Secrets not found
   - Detailed tables with file paths and line numbers

2. **Artifacts**: JSON file containing detailed results for each alert location

3. **Workflow Status**: 
   - ✅ Success: No secrets found
   - ❌ Failure: Secrets still present

## Example Output

```
## Secret Scanning Alert Summary for branch: main

- **Total locations checked**: 3
- **Secrets still present**: 1
- **Secrets not found**: 2

### 🚨 Secrets Still Present

| Alert # | Secret Type | File Path | Lines | Status |
|---------|-------------|-----------|-------|--------|
| 42 | GitHub Personal Access Token | src/config.js | 15-15 | ⚠️ Present |

### ✅ Secrets Not Found

| Alert # | Secret Type | File Path | Reason |
|---------|-------------|-----------|--------|
| 38 | API Key | old-config.yml | file_not_found |
| 41 | Database Password | .env.example | removed |
```

## How It Works

1. **Alert Retrieval**: Uses GitHub API to fetch all secret scanning alerts
2. **Location Processing**: For each alert, gets the file path and line/column information
3. **Content Comparison**: Downloads the current file content from the specified branch
4. **Secret Extraction**: Extracts the specific lines and columns where secrets were detected
5. **Comparison**: Checks if the secret content still exists at those locations
6. **Reporting**: Generates detailed summaries and saves results as artifacts

## Customization

### Modify Alert States

By default, the workflow checks all open alerts. You can modify the alert retrieval to include different states:

```bash
# Include resolved alerts
gh api repos/$REPO/secret-scanning/alerts?state=resolved

# Include all alerts
gh api repos/$REPO/secret-scanning/alerts?state=all
```

### Custom Branch Logic

You can modify the workflow to check multiple branches or implement custom branch selection logic.

### Enhanced Reporting

The workflow generates JSON artifacts that can be processed by other tools or workflows for custom reporting.

## Security Considerations

- The workflow does not expose actual secret values in logs
- Results are saved as artifacts with limited retention (30 days)
- Requires appropriate repository permissions
- Should be run in trusted environments only

## Troubleshooting

### Common Issues

1. **No alerts found**: Ensure secret scanning is enabled for the repository
2. **Permission denied**: Check that the workflow has `security-events: read` permission
3. **File not found**: The file may have been deleted or moved since the alert was created
4. **Rate limiting**: GitHub API has rate limits; the workflow includes proper pagination

### Debugging

Enable debug logging by setting the `ACTIONS_STEP_DEBUG` secret to `true` in your repository settings.

## Contributing

This workflow can be extended to support additional features:
- Integration with notification systems
- Custom secret pattern matching
- Multi-repository scanning
- Integration with security dashboards

## License

This project is available under the MIT License.
