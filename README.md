# RoboLancers Management

This repository automatically creates and configures new robot code repositories with the basic setup needed to start programming.

## How to Create a New Repository

### Step 1: First Time Setup (Ask a Mentor)

This automation uses a GitHub App called **`robolancers-repository-manager`** to create and configure repositories. A mentor needs to follow the [GitHub App Setup Guide](docs/GITHUB_APP_SETUP.md) one time to enable this automation.

**Note:** If you see authentication errors, the private key may have expired. Ask the app owner to generate a new key and update the secrets.

### Step 2: Create Your Repository

1. Go to **Actions** tab (at the top of this page)
2. Click **Setup Repository** on the left
3. Click the **Run workflow** button
4. Fill in the form:
   - **Team number**: Choose `321` or `427`
   - **Competition name**: Type the competition name (like `Reefscape`) or `offseason`
   - **Year**: Leave blank to use the current year, or type a year like `2025`
5. Click **Run workflow**

Wait a few minutes and your new repository will be ready to use!

### Step 3: Speed Up Codespaces (Optional but Recommended)

Make Codespaces start faster (1 minute instead of 10 minutes):

1. Go to your new repository
2. Click **Settings** at the top
3. Click **Codespaces** on the left
4. Click **Set up prebuild**
5. Choose:
   - **Branch**: `main`
   - **Configuration**: `.devcontainer/devcontainer.json`
6. Click **Create**

## What You Get

Your new repository is set up with everything you need to start coding:

- ✅ **Basic Robot Template** - Starter files so you can begin writing code immediately
- ✅ **Development Environment** - Click to start, no software installation needed
- ✅ **Auto-Formatting** - Code formats automatically when you save
- ✅ **Quality Checks** - Makes sure code compiles before merging
- ✅ **Code Review Setup** - Requires teammate approval before changes go to main

**You write all the robot code** - this just gives you the tools and starting point!

## More Information

- 📖 [GitHub App Setup](docs/GITHUB_APP_SETUP.md) - For mentors: one-time setup
- 📖 [What's Included](docs/TEMPLATES.md) - See the starter template files
- 📖 [Advanced Settings](docs/CONFIGURATION.md) - For experienced users
