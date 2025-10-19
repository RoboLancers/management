# Git Hooks

This directory contains git hooks that enforce code quality and commit standards for RoboLancers repositories.

## Setup

Git hooks are automatically configured when you open the repository in a devcontainer. The devcontainer's `postCreateCommand` runs:

```bash
git config core.hooksPath .githooks
```

To manually enable these hooks in your local repository:

```bash
git config core.hooksPath .githooks
```

## Available Hooks

### pre-commit

Automatically formats code using Spotless before each commit.

**What it does:**

1. Runs `./gradlew spotlessApply` on staged files
2. Automatically formats your code to match team standards
3. Re-stages the formatted files
4. Allows commit to proceed (even if there are issues, but warns you)

**Benefits:**

- 🎨 **Consistent formatting** across the entire codebase
- ⚡ **Automatic fixes** - no manual formatting needed
- 🤝 **Team alignment** - everyone uses the same style
- 🔄 **No review friction** - formatting never blocks PRs

**What is Spotless?**

Spotless is a Gradle plugin that enforces code formatting standards. It can format:

- Java code
- JSON files
- Markdown files
- And more!

The hook runs Spotless automatically, so you don't have to remember to format your code manually.

**Note:** The hook is configured to allow commits even if Spotless finds issues (with a warning). This prevents blocking work, but you should fix any syntax errors before pushing.

### commit-msg

Validates commit messages to follow the Conventional Commits specification.

**Required format:**

```
type(scope): description
```

**Allowed types:**

- `feat` - A new feature
- `fix` - A bug fix
- `docs` - Documentation changes
- `breaking` - Breaking changes

**Examples of valid commit messages:**

```bash
feat(robot): add autonomous navigation
fix(vision): correct camera calibration
docs(readme): update installation instructions
breaking(api): change command structure
```

**Examples of invalid commit messages:**

```bash
❌ "updated code"
❌ "feat fixed the bug"
❌ "feat: add feature" (missing scope)
❌ "feature(scope): description" (wrong type)
```

**Testing the commit-msg hook:**

To test if a commit message format will pass:

```bash
echo "feat(robot): test message" | ./.githooks/commit-msg /dev/stdin
```

Or test with an actual commit message file:

```bash
echo "feat(robot): test message" > test-commit.txt
./.githooks/commit-msg test-commit.txt
```

**Testing the pre-commit hook:**

You can manually run the pre-commit hook:

```bash
./.githooks/pre-commit
```

Or test Spotless directly:

```bash
./gradlew spotlessCheck  # Check for formatting issues
./gradlew spotlessApply  # Automatically fix formatting
```

## Why Use These Hooks?

### Conventional Commits (commit-msg)

Using conventional commits provides several benefits:

1. **Automated Changelogs**: Generate changelogs automatically from commit history
2. **Semantic Versioning**: Automatically determine version bumps (major/minor/patch)
3. **Clear History**: Easy to understand what changed and why
4. **Better Collaboration**: Team members know what to expect from each commit
5. **Automated Releases**: Can trigger CI/CD based on commit types

### Automatic Formatting (pre-commit)

Using Spotless for automatic formatting provides:

1. **Zero Configuration**: Set it and forget it
2. **Consistent Style**: No debates about formatting
3. **Reduced PR Comments**: No "fix formatting" comments
4. **Focus on Logic**: Spend time on code quality, not style
5. **Team Onboarding**: New members automatically follow standards

## Bypassing Hooks (Not Recommended)

If you absolutely need to bypass the hooks (e.g., during an emergency), you can use:

```bash
git commit --no-verify -m "your message"
```

⚠️ **Warning**: Bypassing hooks should be rare and only for exceptional cases.

## Adding More Hooks

To add additional hooks to robot code repositories:

1. Create a new hook file in `templates/githooks/` in the management repo (e.g., `pre-push`)
2. Make it executable: `chmod +x templates/githooks/your-hook`
3. Update this README to document the new hook
4. Run the setup workflow to deploy the updated hooks to target repositories

## Common Hook Types

- **pre-commit**: Runs before a commit is created (e.g., linting, formatting)
- **commit-msg**: Validates commit message format (current)
- **pre-push**: Runs before pushing (e.g., run tests)
- **post-checkout**: Runs after checking out a branch
- **post-merge**: Runs after a merge

## Troubleshooting

### Hooks not running

1. Verify hooks path is configured:

   ```bash
   git config core.hooksPath
   ```

   Should output: `.githooks`

2. Ensure the hooks are executable:

   ```bash
   ls -l .githooks/
   ```

   Should show: `-rwxr-xr-x` for both `pre-commit` and `commit-msg`

3. If not executable:
   ```bash
   chmod +x .githooks/pre-commit .githooks/commit-msg
   ```

### Pre-commit hook: Spotless not found

If you see "command not found" or Gradle errors:

1. Ensure you have a `build.gradle` file with Spotless configured
2. Check that Gradle wrapper exists: `./gradlew`
3. If Gradle wrapper is missing, initialize it:
   ```bash
   gradle wrapper
   ```

### Pre-commit hook: Takes too long

The pre-commit hook runs Spotless on your code, which can take a few seconds. This is normal. If it takes more than 10-15 seconds, you might want to configure Spotless to only check changed files.

### Hook fails on valid message

Check if there are hidden characters or encoding issues in your commit message. Try composing the message in a plain text editor.

## Resources

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Hooks Documentation](https://git-scm.com/docs/githooks)
- [Semantic Versioning](https://semver.org/)
