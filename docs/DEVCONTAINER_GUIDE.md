# Devcontainer Configurations

This document explains the different devcontainer configurations used in the RoboLancers organization.

## Two Types of Devcontainers

### 1. Management Repository (This Repo)

**Purpose**: Documentation, automation, and workflow management

**Configuration**: `.devcontainer/devcontainer.json` (in this repo)

**Features**:

- Lightweight Ubuntu base image
- Git tools
- GitHub Actions YAML extension
- No Java/build tools (not needed)

**Use Case**: For editing workflows, documentation, and managing the automation system itself.

```jsonc
{
  "name": "Management Repository",
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu-24.04",
  "features": {
    "ghcr.io/devcontainers/features/git:1": {}
  },
  "postCreateCommand": "git config core.hooksPath .githooks"
}
```

### 2. Robot Code Repositories (Target Repos)

**Purpose**: FRC robot code development

**Configuration**: Deployed automatically by the setup workflow

**Features**:

- WPILib 2025.2.1 (FRC development suite)
- Java 17 with Gradle and Maven
- Desktop environment (VNC on ports 5901/6080)
- VS Code extensions:
  - WPILib extension
  - Java Pack
  - Gradle support
- Git hooks automatically configured

**Use Case**: For developing, building, and deploying FRC robot code.

```jsonc
{
  "name": "WPILib Robot Development",
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu-24.04",
  "postCreateCommand": "cd /home/vscode && wget 'https://github.com/wpilibsuite/vscode-wpilib/releases/download/v2025.2.1/vscode-wpilib-2025.2.1.vsix' && git config core.hooksPath .githooks",
  "features": {
    "ghcr.io/devcontainers/features/java:1": {
      "version": "17",
      "installGradle": "true",
      "installMaven": "true"
    },
    "desktop-lite": {
      "password": "vscode",
      "webPort": "6080",
      "vncPort": "5901"
    }
  },
  "runArgs": ["--shm-size=1g"],
  "forwardPorts": [6080, 5901],
  "customizations": {
    "vscode": {
      "extensions": [
        "/home/vscode/vscode-wpilib-2025.2.1.vsix",
        "vscjava.vscode-java-pack",
        "vscjava.vscode-gradle"
      ]
    }
  }
}
```

## Why Two Different Configurations?

### Management Repo Needs:

- ✅ Fast startup time
- ✅ Minimal resource usage
- ✅ Text editing and git operations
- ❌ No need for Java/build tools
- ❌ No GUI applications

### Robot Code Repo Needs:

- ✅ Full Java development environment
- ✅ WPILib tools for FRC development
- ✅ GUI support for simulators and tools
- ✅ Gradle for building robot code
- ✅ Desktop environment for WPILib utilities
- ✅ All FRC-specific VS Code extensions

## Accessing the Desktop Environment

When working in a robot code repository devcontainer:

1. **Via noVNC (Browser)**:

   - Open `http://localhost:6080` in your browser
   - Password: `vscode`
   - Use for WPILib GUI tools, simulators, etc.

2. **Via VNC Client**:
   - Connect to `localhost:5901`
   - Password: `vscode`
   - Better performance for intensive GUI work

## Updating Configurations

### To Update Management Repo Devcontainer:

Edit `.devcontainer/devcontainer.json` in this repository directly.

### To Update Robot Code Devcontainer:

Edit `templates/.devcontainer/devcontainer.json` in this repository.

The workflow automatically copies this template file to target repositories. After updating the template, run the setup workflow again on target repositories to deploy changes.

## WPILib Version Updates

To update WPILib version for robot code repos:

1. Check latest release: https://github.com/wpilibsuite/vscode-wpilib/releases
2. Update the version in `templates/.devcontainer/devcontainer.json`:
   ```jsonc
   "postCreateCommand": "cd /home/vscode && wget 'https://github.com/wpilibsuite/vscode-wpilib/releases/download/vX.X.X/vscode-wpilib-X.X.X.vsix' && git config core.hooksPath .githooks",
   ```
3. Update both the download URL in `postCreateCommand` and the extension path in the `extensions` array:
   ```jsonc
   "extensions": [
     "/home/vscode/vscode-wpilib-X.X.X.vsix",
     ...
   ]
   ```
4. Run the setup workflow on target repositories to deploy the updated configuration

## Troubleshooting

### Management Repo Issues

**Slow startup**:

- Should be very fast (< 30 seconds)
- If slow, check Docker/Codespaces resources

### Robot Code Repo Issues

**WPILib extension not loading**:

- Check the extension was downloaded in postCreateCommand
- Look at devcontainer logs for wget errors
- Verify the URL is correct for the version

**Desktop/VNC not accessible**:

- Check ports 6080 and 5901 are forwarded
- Verify `desktop-lite` feature is installed
- Try restarting the container

**Gradle build fails**:

- Ensure Java 17 is installed: `java -version`
- Check Gradle wrapper exists: `./gradlew --version`
- Verify `gradlew` has execute permissions

## Best Practices

1. **Don't mix configurations**: Keep management repo lightweight
2. **Test before deploying**: Test devcontainer changes on a single repo first
3. **Document changes**: Update this file when changing configurations
4. **Version pinning**: Pin WPILib versions for consistency across team
5. **Resource awareness**: Desktop environment needs more resources than headless

## Resources

- [Dev Containers Documentation](https://containers.dev/)
- [WPILib Documentation](https://docs.wpilib.org/)
- [VS Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers)
- [GitHub Codespaces](https://docs.github.com/en/codespaces)
