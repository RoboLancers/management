# Configuration Reference

Advanced customization options for repository automation.

## Branch Protection Rules

The workflow automatically configures comprehensive branch protection. To customize, edit `.github/workflows/setup-repository.yml` in the "Apply Branch Protection Rules" step.

### Default Configuration

```json
{
  "required_status_checks": {
    "strict": true,
    "contexts": ["spotless", "build"]
  },
  "enforce_admins": true,
  "required_pull_request_reviews": {
    "dismiss_stale_reviews": true,
    "require_code_owner_reviews": true,
    "required_approving_review_count": 1
  },
  "required_linear_history": true,
  "allow_force_pushes": false,
  "allow_deletions": false,
  "required_conversation_resolution": true
}
```

### Branch Protection Options

| Option                             | Default | Description                                            |
| ---------------------------------- | ------- | ------------------------------------------------------ |
| `required_approving_review_count`  | 1       | Number of required approving reviews                   |
| `dismiss_stale_reviews`            | true    | Dismiss approvals when new commits are pushed          |
| `require_code_owner_reviews`       | true    | Require review from code owners (if CODEOWNERS exists) |
| `required_conversation_resolution` | true    | Require all PR conversations to be resolved            |
| `required_linear_history`          | true    | Prevent merge commits                                  |
| `allow_force_pushes`               | false   | Allow force pushes to protected branch                 |
| `allow_deletions`                  | false   | Allow branch deletion                                  |
| `enforce_admins`                   | true    | Apply rules to administrators                          |

### Common Customizations

**Require 2 reviewers:**

```json
"required_pull_request_reviews": {
  "required_approving_review_count": 2
}
```

**Add more required CI checks:**

```json
"required_status_checks": {
  "strict": true,
  "contexts": ["spotless", "build", "test", "deploy"]
}
```

**Allow force pushes (not recommended):**

```json
"allow_force_pushes": true
```

**Restrict who can push:**

```json
"restrictions": {
  "users": ["username1"],
  "teams": ["team-slug"],
  "apps": ["app-name"]
}
```

## Repository Settings

Configure repository features and merge options. Edit the "Apply Repository Settings" step in the workflow.

### Default Configuration

| Setting                       | Default    | Description                                       |
| ----------------------------- | ---------- | ------------------------------------------------- |
| `has_issues`                  | true       | Enable issue tracking                             |
| `has_projects`                | true       | Enable project boards                             |
| `has_wiki`                    | false      | Enable wiki (disabled - use README/docs instead)  |
| `has_discussions`             | true       | Enable GitHub Discussions                         |
| `allow_squash_merge`          | true       | Allow squash merging (default merge method)       |
| `allow_merge_commit`          | false      | Allow merge commits (disabled for linear history) |
| `allow_rebase_merge`          | true       | Allow rebase merging                              |
| `allow_auto_merge`            | true       | Enable auto-merge when PR is approved             |
| `delete_branch_on_merge`      | true       | Auto-delete branches after merge                  |
| `allow_update_branch`         | true       | Allow updating PR branch with base branch         |
| `squash_merge_commit_title`   | `PR_TITLE` | Use PR title for squash commit                    |
| `squash_merge_commit_message` | `PR_BODY`  | Use PR body for squash commit                     |

### Common Customizations

**Enable wiki:**

```yaml
-f has_wiki=true
```

**Allow merge commits:**

```yaml
-f allow_merge_commit=true
```

**Keep branches after merge:**

```yaml
-f delete_branch_on_merge=false
```

**Disable auto-merge:**

```yaml
-f allow_auto_merge=false
```

## Advanced Features

### CODEOWNERS

Create a `.github/CODEOWNERS` file in your robot repository to automatically request reviews from specific people:

```
# Default owners for everything
* @RoboLancers/programming-leads

# Specific subsystems
/src/main/java/frc/robot/subsystems/drivetrain/ @username1
/src/main/java/frc/robot/subsystems/arm/ @username2

# Configuration files
*.json @RoboLancers/mentors
build.gradle @RoboLancers/mentors
```

### Required Status Checks

The default configuration requires two CI checks to pass:

- `spotless` - Code formatting check
- `build` - Gradle build and tests

To add more checks, modify the workflow:

```json
"required_status_checks": {
  "strict": true,
  "contexts": [
    "spotless",
    "build",
    "integration-tests",
    "security-scan"
  ]
}
```

### Push Restrictions

Limit who can push directly to protected branches:

```json
"restrictions": {
  "users": ["mentor1", "mentor2"],
  "teams": ["programming-leads"],
  "apps": []
}
```

**Note:** Empty `restrictions` (null) means anyone with write access can push (subject to other protection rules like required reviews).

## Troubleshooting

### "Resource not accessible by integration"

The GitHub App needs `Administration: Read and write` permission. See [GitHub App Setup Guide](GITHUB_APP_SETUP.md).

### "Required status check does not exist"

The status check name must exactly match what your CI workflow reports. Check:

1. Workflow job names in `.github/workflows/build.yml`
2. Status check contexts in branch protection JSON

### "Required reviews not satisfied"

Ensure:

1. PR has the required number of approvals
2. Reviews are from users with write access
3. Reviews aren't dismissed by new commits (if `dismiss_stale_reviews: true`)

### Branch Protection Not Applied

Check:

1. Target branch exists with at least one commit
2. GitHub App has admin permissions on repository
3. Workflow completed successfully (check Actions tab)

## API Rate Limits

GitHub API has rate limits:

- **Authenticated requests**: 5,000 per hour
- **GitHub App**: Higher limits depending on installations

If you hit rate limits:

- Wait for the limit to reset (check response headers)
- Spread out workflow runs
- Consider batching repository setups

## Further Reading

- [GitHub Branch Protection API](https://docs.github.com/en/rest/branches/branch-protection)
- [GitHub Repository Settings API](https://docs.github.com/en/rest/repos/repos#update-a-repository)
- [CODEOWNERS Documentation](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
