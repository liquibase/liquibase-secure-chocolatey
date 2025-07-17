![CI](https://github.com/liquibase/liquibase-chocolatey/actions/workflows/deploy-package.yml/badge.svg)
[![Chocolatey](https://img.shields.io/chocolatey/v/liquibase.svg)](https://chocolatey.org/packages/liquibase)
[![Chocolatey](https://img.shields.io/chocolatey/dt/liquibase.svg)](https://chocolatey.org/packages/liquibase)


# Liquibase chocolatey package

The aim of this project is to make the release process of liquibase package as easy and fast as possible so it will stay as close as possible from Liquibase release pipe.

## Installing Liquibase with Chocolatey

1. **Open Command Prompt or PowerShell**: Open Command Prompt or `PowerShell` with administrator privileges. You can do this by right-clicking on the Command Prompt/PowerShell icon and selecting `Run as administrator`.
2. **Install Liquibase**: Type the following command and press `Enter`:

  ```bash
  choco install liquibase
  ```

3. **Verify Installation**: Once the installation is complete, you can verify `Liquibase` installation by running:

  ```bash
  liquibase --version
  ```

  This command should display the `Liquibase` version installed on your system.

## Upgrading Liquibase with Chocolatey

1. **Check for Updates:**: Before upgrading, it's a good practice to check if there are updates available for `Liquibase`. Run the following command:

  ```bash
  choco upgrade liquibase --noop
  ```

  This command will simulate the upgrade process without actually performing it.
   
2. **Upgrade Liquibase**: If there are updates available and you're ready to upgrade, run:

  ```bash
  choco upgrade liquibase
  ```

3. **Verify Upgrade**: After the upgrade process is complete, verify that `Liquibase` has been upgraded successfully by running:

  ```bash
  liquibase --version
  ```

  This command should display the `Liquibase` version installed on your system.

## Removing Liquibase with Chocolatey

1. **Uninstall Liquibase:**: To remove `Liquibase` from your system, run the following command:

  ```bash
  choco uninstall liquibase
  ```

  Confirm the uninstallation when prompted.
   
2. **Verify Removal**:  After the uninstallation process is complete, verify that `Liquibase` has been removed by running:

  ```bash
  liquibase --version
  ```

## Additional Notes

* **Chocolatey Installation**: If you haven't installed `Chocolatey` yet, you can do so by following the instructions on the `Chocolatey` website: [Chocolatey Installation Guide](https://chocolatey.org/install).
* **Permissions**: Make sure you have the necessary permissions (e.g., administrator rights) to install, upgrade, or remove software on your system.

By following these steps, you should be able to easily manage `Liquibase` using `Chocolatey` on your system.

## Continuous build

The build and deploy process is triggered from [liquibase/liquibase](https://github.com/liquibase/liquibase) every time a new `liquibase` version is released. Check [deploy-package.yml](.github/workflows/deploy-package.yml) for more details.

## Chocolatey Release Tracking

This repository includes automated release tracking for Chocolatey packages. The [chocolatey-release-tracking.yml](.github/workflows/chocolatey-release-tracking.yml) workflow automatically checks if newly deployed Liquibase version is available on Chocolatey and sends notifications.

### How it works

* Automatically runs after the "Choco Build and Deploy" workflow completes successfully
* Also runs on a scheduled basis to continue checking packages that may be in processing/waiting status
* Extracts the version number from the deployment workflow
* Checks if the version is available at `https://community.chocolatey.org/packages/liquibase/<version>`
* Detects when packages are in "waiting" status (pending moderator approval)
* Sends Slack notifications with the availability status

Since Chocolatey validation and approval can take time, the scheduled runs ensure continued monitoring until packages are fully available.

### Notification Types

The workflow sends one of three types of notifications:

* **✅ Available**: Package is live on Chocolatey with installation instructions
* **⏳ Processing**: Package was deployed but not yet available (may still be processing)
* **❌ Error**: There was an issue checking the package availability

No manual setup is required - the tracking runs automatically after each successful deployment.
