# Migrating from Jenkins to GitHub Actions: A Step-by-Step Guide

GitHub Actions Importer helps you plan, test, and automate your migration to GitHub Actions

## Prerequisites
- Ensure you have admin access to both Jenkins and GitHub.
- Install the GitHub CLI (`gh`), and authenticate using `gh auth login`.
- Install `gh actions-importer` by running:
  ```sh gh extension install github/gh-actions-importer
  ```

## Required Permissions
### GitHub
- `repo` scope for repository access
- `workflow` scope for managing GitHub Actions workflows
- `admin:repo_hook` for creating webhooks

### Jenkins
- Read access to Jenkins job configurations
- Admin access for API tokens

## Migration Steps
### 1. Audit Jenkins Workflows
Run the following command to audit Jenkins workflows before migration:
```sh
gh actions-importer audit jenkins \
  --jenkins-instance-url http://localhost:8080 \
  --jenkins-username "admin" \
  --jenkins-access-token "<JENKINS_API_TOKEN>" \
  --github-access-token "<GITHUB_PERSONAL_ACCESS_TOKEN>" \
  --output-dir tmp/audit
```

### 2. Perform a Dry Run
Run the dry-run command to simulate migration:
```sh
gh actions-importer dry-run jenkins \
  --jenkins-instance-url http://localhost:8080 \
  --jenkins-username "admin" \
  --jenkins-access-token "<JENKINS_API_TOKEN>" \
  --github-access-token "<GITHUB_PERSONAL_ACCESS_TOKEN>" \
  --source-url http://localhost:8080/job/MyPipeline/ \
  --output-dir tmp/dry-run
```

### 3. Migrate Jenkins Jobs to GitHub Actions
Execute the following command to migrate Jenkins jobs:
```sh
gh actions-importer migrate jenkins  \
  --jenkins-instance-url http://localhost:8080  \
  --jenkins-username "admin" \
  --jenkins-access-token "<JENKINS_API_TOKEN>" \
  --github-access-token "<GITHUB_PERSONAL_ACCESS_TOKEN>"  \
  --source-url http://localhost:8080/job/MyPipeline/  \
  --target-url https://github.com/mayurdevops24/jenkins-to-github \
  --output-dir tmp/migration
```

### 4. Verify and Test
- Navigate to your GitHub repository and check the `.github/workflows` folder.
- Manually trigger a workflow run to verify successful migration.
- Check GitHub Actions logs for any errors or warnings.

## Troubleshooting
- Ensure the `gh actions-importer` extension is correctly installed.
- Double-check API tokens and permissions.
- Review the logs generated in `tmp/migration` for errors.
- Validate Jenkins job configurations before migration.

## Cleanup & Remove Jenkins (Optional)

gh actions-importer delete jenkins --jenkins-instance-url http://localhost:8080 --jenkins-username "admin" --jenkins-password "11a8f99f6dd777a2fd5fb4ff54ed570eb1"

## Summary
This guide ensures a smooth migration from Jenkins to GitHub Actions by following a structured approach: auditing workflows, performing a dry run, migrating jobs, and verifying the results. 



