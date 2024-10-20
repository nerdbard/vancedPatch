# vancedPatch

## Overview

`vancedPatch` is a GitHub Actions workflow designed to automatically build ReVanced APKs from the APK Mirror's service. It can be triggered manually or scheduled to run daily. The workflow builds the specified package as an APK and sends it to a designated Telegram chat.

## Workflow Configuration

### Triggers

- **Manual Trigger**: Allows the workflow to be executed from the GitHub Actions interface.
- **Scheduled Trigger**: Runs daily at 5:30 AM UTC.

### Jobs

#### `run`

- **Environment**: Ubuntu
- **Permissions**: Full write permissions

#### Steps

1. **Setup Java**
   - Uses `actions/setup-java@v4` to install Zulu OpenJDK 17.

2. **Checkout Repository**
   - Utilizes `actions/checkout@v4` to check out the repository and its submodules.

3. **Update Configuration**
   - Updates the configuration file if the workflow is triggered from another CI workflow.

4. **Get Next Version Code**
   - Retrieves the next version code from the latest GitHub release using the `gh` CLI.

5. **Build Modules/APKs**
   - Runs the build script with the configuration file to generate modules and APKs.

6. **Get Build Output**
   - Captures build logs and stores them for future reference.

7. **Upload Modules to Release**
   - Uses `svenstaro/upload-release-action@v2` to upload the generated modules and APKs to a GitHub release.

8. **Update Changelog and Magisk Update JSON**
   - Updates the changelog and creates a JSON file for Magisk modules based on the build output.

9. **Commit Changes**
   - Automatically commits changes to the `update` branch using `stefanzweifel/git-auto-commit-action@v5`.

10. **Report to Telegram**
    - Sends a notification with download links for the new builds to a specified Telegram chat.

## Secrets Required

- **TELEGRAM_FROM**: Your Telegram bot token for sending notifications.
- **TELEGRAM_TO**: The chat ID where notifications will be sent.

## Usage

1. **Manual Execution**:
   - Go to the "Actions" tab in your GitHub repository.
   - Select `Build Modules` and click on "Run workflow."

2. **Automatic Execution**:
   - The workflow runs automatically every day at 5:30 AM UTC.

## Error Handling

- The workflow includes error handling for API calls, file downloads, and other critical steps. If any step fails, an error message will be logged, and the workflow will exit with a non-zero status.

## Contributions

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

For any questions or feedback, please open an issue in the repository!
