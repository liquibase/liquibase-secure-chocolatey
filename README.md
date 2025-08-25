![CI](https://github.com/liquibase/liquibase-secure-chocolatey/actions/workflows/deploy-package.yml/badge.svg)
[![Chocolatey](https://img.shields.io/chocolatey/v/liquibase-secure.svg)](https://chocolatey.org/packages/liquibase-secure)
[![Chocolatey](https://img.shields.io/chocolatey/dt/liquibase-secure.svg)](https://chocolatey.org/packages/liquibase-secure)

# Liquibase Secure chocolatey package

The aim of this project is to make the release process of Liquibase Secure package as easy and fast as possible so it will stay as close as possible from LiquibaseSecure release pipe.

## Installing Liquibase Secure with Chocolatey

1.**Open Command Prompt or PowerShell**: Open Command Prompt or `PowerShell` with administrator privileges. You can do this by right-clicking on the Command Prompt/PowerShell icon and selecting `Run as administrator`.
2. **Install Liquibase Secure**: Type the following command and press `Enter`:

  ```bash
  choco install liquibase-secure
  ```

3.**Verify Installation**: Once the installation is complete, you can verify `Liquibase Secure` installation by running:

  ```bash
  liquibase --version
  ```

  This command should display the `Liquibase Secure` version installed on your system.

## Upgrading Liquibase Secure with Chocolatey

1. **Check for Updates:**: Before upgrading, it's a good practice to check if there are updates available for `Liquibase Secure`. Run the following command:

  ```bash
  choco upgrade liquibase-secure --noop
  ```

  This command will simulate the upgrade process without actually performing it.

2.**Upgrade Liquibase Secure**: If there are updates available and you're ready to upgrade, run:

  ```bash
  choco upgrade liquibase-secure
  ```

3.**Verify Upgrade**: After the upgrade process is complete, verify that `Liquibase Secure` has been upgraded successfully by running:

  ```bash
  liquibase --version
  ```

  This command should display the `Liquibase Secure` version installed on your system.

## Removing Liquibase Secure with Chocolatey

1. **Uninstall Liquibase Secure:**: To remove `Liquibase Secure` from your system, run the following command:

  ```bash
  choco uninstall liquibase-secure
  ```

  Confirm the uninstallation when prompted.

2.**Verify Removal**:  After the uninstallation process is complete, verify that `Liquibase Secure` has been removed by running:

  ```bash
  liquibase --version
  ```

## Package vs Executable Naming

**Important Note**: While the Chocolatey package is named `liquibase-secure`, the executable that gets installed is still called `liquibase`. This means:

- **Installation**: `choco install liquibase-secure`
- **Verification**: `liquibase --version`
- **Usage**: All standard `liquibase` commands work normally

### Why this approach?

1. **Same Executable**: Liquibase Secure is the same Liquibase executable, just the enterprise/secure edition with enhanced security features
2. **Seamless Experience**: Users can switch between community and secure editions without changing their workflow or commands  
3. **Standard Practice**: This follows typical Chocolatey patterns where package names can differ from executable names
4. **Consistency**: All Liquibase commands and documentation remain the same regardless of which edition you install

The package name `liquibase-secure` distinguishes it from the community edition during installation, but once installed, you use the familiar `liquibase` command.

## Additional Notes

* **Chocolatey Installation**: If you haven't installed `Chocolatey` yet, you can do so by following the instructions on the `Chocolatey` website: [Chocolatey Installation Guide](https://chocolatey.org/install).
* **Permissions**: Make sure you have the necessary permissions (e.g., administrator rights) to install, upgrade, or remove software on your system.

By following these steps, you should be able to easily manage `Liquibase Secure` using `Chocolatey` on your system.

## Purpose

This repository serves as the dedicated Chocolatey packaging system for **Liquibase Secure**, the enterprise-grade edition of Liquibase with enhanced security features. As part of the Liquibase 5.0 distribution separation and secure rebranding initiative, this repository ensures that Windows users can easily install and manage Liquibase Secure through the familiar Chocolatey package manager.

### Key Objectives:

- **Separation of Distributions**: Provide a distinct Chocolatey package (`liquibase-secure`) separate from the community edition
- **Enterprise Security**: Package the secure edition with enhanced security controls and enterprise integrations  
- **Automated Deployment**: Maintain synchronized releases with the main Liquibase release pipeline
- **Quality Assurance**: Ensure package validation, testing, and monitoring throughout the deployment process

## CI/CD Pipeline Architecture

The Liquibase Secure Chocolatey package deployment follows a sophisticated CI/CD pipeline designed for reliability, automation, and monitoring. The architecture consists of multiple interconnected workflows that handle building, deploying, and tracking package releases.

```
┌─────────────────┐    ┌──────────────────┐    ┌────────────────────┐
│   Liquibase     │    │    Build &       │    │    Chocolatey      │
│   Release       │───▶│    Deploy        │───▶│    Community       │
│   Trigger       │    │    Workflow      │    │    Platform        │
└─────────────────┘    └──────────────────┘    └────────────────────┘
                                │                        │
                                ▼                        │
                       ┌──────────────────┐              │
                       │   Placeholder    │              │
                       │   Branch         │              │
                       │   Management     │              │
                       └──────────────────┘              │
                                                         │
                                                         ▼
┌─────────────────┐    ┌──────────────────┐    ┌────────────────────┐
│   Slack         │◀───│    Release       │◀───│    Scheduled       │
│   Notifications │    │    Tracking      │    │    Monitoring      │
└─────────────────┘    └──────────────────┘    └────────────────────┘
```

## Workflow Architecture Overview

### 1. **Primary Deployment Pipeline** (`deploy-package.yml`)

- **Trigger**: Manual dispatch or workflow call from Liquibase Secure main repository
- **Environment**: Windows-latest with Java 17, Ant, and Chocolatey
- **Process**: Downloads Liquibase Secure release → Generates checksums → Builds package → Tests installation → Publishes to Chocolatey
- **Output**: `liquibase-secure.X.X.X.nupkg` published to Chocolatey Community

### 2. **Release Tracking System** (`chocolatey-release-tracking.yml`)

- **Trigger**: Scheduled runs + post-deployment monitoring
- **Function**: Monitors Chocolatey approval process and package availability
- **Integration**: Slack notifications for status updates
- **Branch Management**: Creates/deletes tracking branches based on approval status

### 3. **Quality Gates and Validation**

- **Checksum Verification**: SHA-512 validation of downloaded packages
- **Local Testing**: Installation verification before publication
- **Dependency Management**: Automatic Temurin 17 JDK installation
- **Error Handling**: Comprehensive logging and debugging output

## Workflow Details Matrix

| Workflow | Trigger | Environment | Key Steps | Outputs | Dependencies |
|----------|---------|-------------|-----------|---------|--------------|
| **deploy-package.yml** | Manual dispatch<br/>Workflow call | Windows-latest | 1. Checkout code<br/>2. Setup Java 17<br/>3. Install tools<br/>4. Build package<br/>5. Test install<br/>6. Publish | `liquibase-secure.nupkg`<br/>Chocolatey publication | Ant, Chocolatey CLI<br/>Temurin 17<br/>GitHub secrets |
| **chocolatey-release-tracking.yml** | Scheduled (daily)<br/>Workflow completion | Ubuntu-latest | 1. Check tracking branch<br/>2. Query Chocolatey API<br/>3. Validate status<br/>4. Send notifications<br/>5. Cleanup branches | Slack notifications<br/>Branch management | xmllint<br/>Chocolatey API<br/>Slack webhooks |

### Key Workflow Parameters

| Parameter | Description | Required | Default | Example |
|-----------|-------------|----------|---------|---------|
| `version` | Liquibase version from pom.xml | ✅ | - | `4.26.0` |
| `dry_run` | Create test package without publishing | ✅ | `false` | `true`/`false` |
| `dry_run_zip_url` | Custom ZIP URL for testing | ❌ | - | `https://example.com/test.zip` |

## Continuous build

The build and deploy process is triggered from [liquibase/liquibase-pro](https://github.com/liquibase/liquibase-pro) every time a new `liquibase-secure` version is released. Check [deploy-package.yml](.github/workflows/deploy-package.yml) for more details.

## Chocolatey Release Tracking

This repository includes automated release tracking for Chocolatey Secure packages. The [chocolatey-release-tracking.yml](.github/workflows/chocolatey-release-tracking.yml) workflow automatically checks if newly deployed Liquibase Secure version is available on Chocolatey and sends notifications.

### How it works

* Automatically runs after the "Choco Build and Deploy - Liquibase Secure" workflow completes successfully
* Also runs on a scheduled basis to continue checking packages that may be in processing/waiting status
* Extracts the version number from the deployment workflow
* Checks if the version is available at `https://community.chocolatey.org/packages/liquibase-secure/<version>`
* Detects when packages are in "waiting" status (pending moderator approval)
* Sends Slack notifications with the availability status

Since Chocolatey validation and approval can take time, the scheduled runs ensure continued monitoring until packages are fully available.

### Notification Types

The workflow sends one of three types of notifications:

* **✅ Available**: Package is live on Chocolatey with installation instructions
* **⏳ Processing**: Package was deployed but not yet available (may still be processing)
* **❌ Error**: There was an issue checking the package availability

No manual setup is required - the tracking runs automatically after each successful deployment.
