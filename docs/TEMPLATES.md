# What's Included in Your Repository

This page explains the starter files and tools that are set up for you. **You still write all the robot code** - these are just the basic files and tools to help you get started!

## Quick Overview

When you create a repository, it comes with:

- **Starter template files** - Basic structure to build on, not complete robot code
- **Development environment** - Tools pre-installed so you can start coding right away
- **Code formatting** - Keeps everyone's code looking consistent
- **Build checks** - Catches errors before they get to the robot
- **Code review setup** - Makes teamwork easier

## What's in the Repository

Here's the starting structure you get:

```
Your Repository/
├── src/main/java/frc/robot/         # ← YOU WRITE YOUR CODE HERE!
│   ├── Robot.java                   # Basic robot structure (you add to this)
│   ├── RobotContainer.java          # Wire things together (you customize this)
│   ├── Constants.java               # Put configuration values here
│   ├── commands/                    # Your robot actions go here (empty to start)
│   └── subsystems/                  # Your robot parts go here (empty to start)
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md    # Form for code reviews
│   └── workflows/build.yml          # Automatic checks
├── .devcontainer/                   # Development environment config
├── .githooks/                       # Auto-formatting setup
└── build.gradle                     # Build configuration
```

**Important:** The `commands/` and `subsystems/` folders are empty - you create all that code yourself!

## What Each Tool Does

### Development Environment (Codespaces)

- **No installation needed** - Everything works in your browser
- **Same setup for everyone** - No "works on my computer" problems
- **All tools included** - WPILib, Java, simulators ready to go

### Auto-Formatting

- **Keeps code consistent** - Everyone's code looks the same
- **Happens automatically** - Just save and commit
- **No style arguments** - Computer handles it

### Build Checks

- **Catches mistakes early** - Finds errors before you deploy to robot
- **Two automatic checks**:
  1. **Formatting check** - Is code formatted?
  2. **Build check** - Does code compile?
- **Must pass to merge** - Protects the main code

### Code Review Setup

- **Template for pull requests** - Standard questions to answer
- **Requires approval** - At least one teammate must review
- **Can't push directly to main** - Prevents accidents

## Starting Your Robot Code

**What you get:**

- Empty folders for commands and subsystems
- Basic Robot.java and RobotContainer.java files
- Your team number already set up

**What you do:**

- Create subsystems for each part of your robot (drivetrain, arm, etc.)
- Create commands for each action your robot takes
- Wire everything together in RobotContainer.java

## Questions?

- **Where do I start coding?** Open the repository in Codespaces and go to `src/main/java/frc/robot/`
- **Is there example code?** Just the basic structure - you write the robot-specific code
- **How do I add a subsystem?** Create a new file in the `subsystems/` folder
- **Why is it mostly empty?** So you can build exactly what your robot needs!
