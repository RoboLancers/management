# RoboLancers Management

This repository handles management, processes, and operations for RoboLancers, including automated repository setup and configuration.

## Repository Automation

Automated repository setup using GitHub Actions workflow dispatch to configure repository settings and branch protections.

## Quick Start

1. **Set up GitHub App** (one-time setup):

   - Follow the [GitHub App Setup Guide](docs/GITHUB_APP_SETUP.md)
   - Create a GitHub App with required permissions
   - Add `APP_ID` and `APP_PRIVATE_KEY` secrets to this repository

2. **Run the workflow**:

   - Go to Actions → "Setup Repository"
   - Select team number (321 or 427)
   - Enter competition name (e.g., `Reefscape` or `offseason`)
   - Enter year (e.g., `2025`)
   - Click "Run workflow"

3. **Done!** Your repository is now configured with:
   - Branch protection rules (with required CI checks)
   - Repository settings
   - Pull request template
   - Devcontainer configuration (Codespaces ready!)
   - Git hooks (pre-commit formatting + commit message validation)
   - CI workflow (automated build and testing)

### Features

This automation configures:

#### 🔒 Branch Protection Rules

- **Required Pull Request Reviews**: At least 1 approval required
- **Required Status Checks**: CI must pass (Spotless + Build)
- **Dismiss Stale Reviews**: Automatically dismiss approvals when new commits are pushed
- **Require Code Owner Reviews**: Enforces CODEOWNERS file if present
- **Required Conversation Resolution**: All PR conversations must be resolved
- **Linear History**: Prevents merge commits
- **No Force Pushes**: Protects against force pushes
- **No Branch Deletion**: Prevents accidental branch deletion

#### ⚙️ Repository Settings

- **Issues**: Enabled
- **Projects**: Enabled
- **Wiki**: Disabled (use README or docs/ instead)
- **Discussions**: Enabled
- **Merge Options**:
  - Squash merging: ✅ Enabled (default)
  - Merge commits: ❌ Disabled
  - Rebase merging: ✅ Enabled
- **Auto-merge**: Enabled
- **Delete Branch on Merge**: Automatically cleanup merged branches
- **Allow Update Branch**: Enable updating PR branches with base branch

#### 📝 Pull Request Template

- **Automatic Setup**: Creates a standardized PR template
- **Structured Format**: Includes description, type of change, and checklist
- **Quality Gates**: Ensures PRs follow best practices with required checks
- **Types**: Bug fix, new feature, breaking change, or work in progress

#### 🐳 Devcontainer Configuration

- **Codespaces Ready**: Pre-configured development environment
- **WPILib 2025.2.1**: FRC robot development tools pre-installed
- **Java 17**: With Gradle and Maven support
- **Desktop Environment**: VNC/noVNC for GUI applications (ports 5901/6080)
- **VS Code Extensions**: WPILib, Java Pack, and Gradle extensions
- **Git Hooks**: Automatically configures git hooks path
- **Consistent Environment**: Same dev environment for all team members

#### 🪝 Git Hooks

- **Pre-commit**: Automatically formats code with Spotless
- **Commit Message Validation**: Enforces Conventional Commits format
- **Automated Quality**: Ensures consistent code style and commit history
- **Required Format**: `type(scope): description`
- **Allowed Types**: `feat`, `fix`, `docs`, `breaking`
- **Auto-configured**: Works automatically in devcontainers
- **Zero Friction**: Formatting happens automatically before each commit

#### 🔄 CI/CD Workflow

- **Automated Testing**: Runs on every push and pull request
- **Parallel Jobs**: Spotless check and build run simultaneously
- **Java 17**: Consistent build environment with Zulu JDK
- **Gradle Build**: Compiles and runs all tests
- **Branch Protection**: Required to pass before merging to main
- **Build Status**: Clear visibility of build health

#### ⚡ Codespaces Prebuilds

- **Automatic Configuration**: Prebuilds are set up automatically for the main branch
- **Faster Startup**: Reduces Codespace creation from 5-10 minutes to ~1 minute
- **Auto-rebuild**: Automatically rebuilds when devcontainer.json or dependencies change
- **Cached Environment**: WPILib installation, Gradle wrapper, and Java setup are pre-built

### Usage

#### Prerequisites

1. **GitHub App Authentication** (Required):

   - A GitHub App must be created and installed for the RoboLancers organization
   - The app needs `Administration`, `Contents`, `Pull requests`, and `Workflows` permissions
   - App credentials (`APP_ID` and `APP_PRIVATE_KEY`) must be stored as repository secrets
   - 📖 **See [GitHub App Setup Guide](docs/GITHUB_APP_SETUP.md) for detailed instructions**

2. **Repository Access**:

   - The GitHub App must be installed with access to the target repository
   - The target repository must already exist

3. **Permissions**:
   - You need the ability to trigger workflows in the `management` repository
   - The GitHub App needs `admin` access to the target repository

#### Running the Workflow

1. Go to **Actions** tab in your repository
2. Select **"Setup Repository"** workflow
3. Click **"Run workflow"** button
4. Fill in the required inputs:
   - **Team number**: Select `321` or `427` from dropdown
   - **Competition name**: Enter the competition name (e.g., `Reefscape`) or `offseason`
   - **Year**: Enter the year in YYYY format (e.g., `2025`)
5. (Optional) Specify the target branch to protect (default: `main`)
6. Click **"Run workflow"** to execute

The workflow will automatically:

- Construct repository name in format: `{team_number}-{competition_name}-{year}` (e.g., `427-Reefscape-2025`)
- Create/configure the repository: `RoboLancers/427-Reefscape-2025`
- Update WPILib preferences with your team number and year
- Apply all configurations listed below

- ✅ Branch protection rules
- ✅ Repository settings
- ✅ Pull request template
- ✅ Devcontainer configuration
- ✅ Git hooks
- ✅ CI workflow
- ✅ WPILib project files (with your team number and year)

#### Command Line Usage

You can also trigger the workflow using GitHub CLI:

```bash
# Apply all settings to a specific repository
gh workflow run setup-repository.yml \
  -f team_number=427 \
  -f competition_name=Reefscape \
  -f year=2025

# Setup for offseason development
gh workflow run setup-repository.yml \
  -f team_number=321 \
  -f competition_name=offseason \
  -f year=2025 \
  -f target_branch=main
```

## Template Structure

All configurations deployed to robot code repositories are stored in the `templates/` directory with the exact same structure they'll have when deployed. This makes it intuitive to understand and update:

```
templates/
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md    # PR template for robot repos
│   └── workflows/
│       └── build.yml                # CI workflow (Spotless + Build)
├── .devcontainer/
│   └── devcontainer.json            # WPILib 2025.2.1 devcontainer config
├── .githooks/
│   ├── pre-commit                   # Spotless formatting hook
│   ├── commit-msg                   # Conventional Commits validation
│   └── README.md                    # Git hooks documentation
├── .vscode/
│   ├── launch.json                  # Debug configurations
│   └── settings.json                # VS Code settings
├── .wpilib/
│   └── wpilib_preferences.json      # Team number and year (auto-updated)
├── gradle/
│   └── wrapper/                     # Gradle wrapper files
├── src/
│   └── main/
│       ├── java/frc/robot/          # Robot source code
│       │   ├── Main.java
│       │   ├── Robot.java
│       │   ├── RobotContainer.java
│       │   ├── Constants.java
│       │   ├── commands/
│       │   │   ├── Autos.java
│       │   │   └── ExampleCommand.java
│       │   └── subsystems/
│       │       └── ExampleSubsystem.java
│       └── deploy/
│           └── example.txt          # Deploy directory info
├── vendordeps/
│   └── README.md                    # Vendor dependencies guide
├── .gitignore                       # WPILib-specific gitignore
├── build.gradle                     # Gradle build config (with Spotless)
├── settings.gradle                  # Gradle settings
├── gradlew                          # Gradle wrapper script (Unix)
├── gradlew.bat                      # Gradle wrapper script (Windows)
└── WPILib-License.md                # WPILib license
```

### Updating Templates

To modify configurations deployed to robot repositories:

1. **Edit template files** in `templates/` directory (maintaining the directory structure)
2. **Run the workflow** on target repositories to deploy updated templates
3. **Templates are copied as-is** - the directory structure is preserved

**Examples:**

- Update PR template: Edit `templates/.github/PULL_REQUEST_TEMPLATE.md`
- Change WPILib version: Edit `templates/.devcontainer/devcontainer.json` and `templates/build.gradle`
- Modify CI checks: Edit `templates/.github/workflows/build.yml`
- Update git hooks: Edit files in `templates/.githooks/`
- Add vendor dependencies: Place JSON files in `templates/vendordeps/`
- Modify code style: Edit Spotless config in `templates/build.gradle`

**Note:** The `templates/.wpilib/wpilib_preferences.json` file uses placeholders (`teamNumber: 0`, `projectYear: "YYYY"`) which are automatically replaced with the workflow inputs when deployed.

### Customization

To customize the settings, edit `.github/workflows/setup-repository.yml` or template files:

#### Branch Protection Customization

Find the "Apply Branch Protection Rules" step and modify the API call parameters:

```yaml
-f required_pull_request_reviews='{"required_approving_review_count":2}' # Change from 1 to 2
```

Common customizations:

- **Required reviewers**: Change `required_approving_review_count`
- **Status checks**: Modify `required_status_checks.contexts` array
  - Default: `["spotless","build"]` (both CI jobs must pass)
  - Add more: `["spotless","build","test","deploy"]`
- **Allow force pushes**: Change `allow_force_pushes` to `true` (not recommended)
- **Restrictions**: Add user/team restrictions for who can push

#### Repository Settings Customization

Find the "Apply Repository Settings" step and modify:

```yaml
-f has_wiki=true              # Enable wiki
-f allow_merge_commit=true    # Enable merge commits
-f delete_branch_on_merge=false  # Keep branches after merge
```

### Configuration Reference

#### Branch Protection Options

| Option                             | Default | Description                                   |
| ---------------------------------- | ------- | --------------------------------------------- |
| `required_approving_review_count`  | 1       | Number of required approving reviews          |
| `dismiss_stale_reviews`            | true    | Dismiss approvals when new commits are pushed |
| `require_code_owner_reviews`       | true    | Require review from code owners               |
| `required_conversation_resolution` | true    | Require all conversations to be resolved      |
| `required_linear_history`          | true    | Prevent merge commits                         |
| `allow_force_pushes`               | false   | Allow force pushes                            |
| `allow_deletions`                  | false   | Allow branch deletion                         |

#### Repository Settings Options

| Option                   | Default | Description                 |
| ------------------------ | ------- | --------------------------- |
| `has_issues`             | true    | Enable issues               |
| `has_projects`           | true    | Enable projects             |
| `has_wiki`               | false   | Enable wiki                 |
| `has_discussions`        | true    | Enable discussions          |
| `allow_squash_merge`     | true    | Allow squash merging        |
| `allow_merge_commit`     | false   | Allow merge commits         |
| `allow_rebase_merge`     | true    | Allow rebase merging        |
| `allow_auto_merge`       | true    | Allow auto-merge            |
| `delete_branch_on_merge` | true    | Auto-delete merged branches |

### Advanced Configuration

#### Adding Required Status Checks

To require specific CI checks to pass before merging:

```yaml
-f required_status_checks='{"strict":true,"contexts":["build","test","lint"]}'
```

#### Setting Up CODEOWNERS

Create a `.github/CODEOWNERS` file:

```
# Default owners for everything
* @your-org/core-team

# Specific paths
/docs/ @your-org/docs-team
*.py @your-org/python-team
```

#### Restrict Push Access

To limit who can push to protected branches:

```yaml
-f restrictions='{"users":["username1"],"teams":["team-slug"],"apps":["app-name"]}'
```

### Troubleshooting

#### GitHub App Issues

For GitHub App setup and authentication issues, see the [GitHub App Setup Guide](docs/GITHUB_APP_SETUP.md#troubleshooting).

Common issues:

- **"Resource not accessible by integration"**: GitHub App needs `Administration: Read and write` permission
- **"Bad credentials"**: Check that `APP_ID` and `APP_PRIVATE_KEY` secrets are set correctly
- **"App is not installed"**: Ensure the GitHub App is installed for the target repository

#### Permission Errors

**Legacy - if not using GitHub App:**

If you see "Resource not accessible by integration":

- Ensure the workflow has `administration: write` permission
- Check that the `GITHUB_TOKEN` has admin access
- Verify you're running the workflow with appropriate permissions

#### API Rate Limits

The GitHub API has rate limits. If you hit them:

- Wait for the rate limit to reset
- Consider using a Personal Access Token with higher limits

#### Branch Not Found

If the target branch doesn't exist:

- Create the branch first
- Or specify an existing branch in the workflow inputs
