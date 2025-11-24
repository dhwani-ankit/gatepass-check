# Dhwani Release Bot Setup Guide

This guide explains how to set up the `dhwani-release-bot` GitHub App for automated semantic releases.

## Overview

The `dhwani-release-bot` is a GitHub App that:
- ✅ Automatically creates releases when code is pushed to `main` or `master` branches
- ✅ Can be triggered manually via comments: `@dhwani-release-bot release`
- ✅ Allows developers to commit normally (no interference with regular commits)
- ✅ Bypasses branch protection rules for releases only

## Prerequisites

- GitHub repository with branch protection enabled
- Admin access to the repository
- Ability to create GitHub Apps

## Step 1: Create a GitHub App

1. Go to your organization or user settings
2. Navigate to **Developer settings** → **GitHub Apps**
3. Click **New GitHub App**
4. Configure the app with these settings:

   **Basic Information:**
   - **GitHub App name**: `dhwani-release-bot`
   - **Homepage URL**: Your repository URL
   - **Webhook**: Leave unchecked (we're using GitHub Actions)

   **Permissions:**
   - **Repository permissions:**
     - Contents: **Read and write**
     - Issues: **Read and write**
     - Pull requests: **Read and write**
     - Metadata: **Read-only** (always enabled)
   
   **Where can this GitHub App be installed?**
   - Select **Only on this account** or **Any account** based on your needs

5. Click **Create GitHub App**

## Step 2: Install the GitHub App

1. After creating the app, click **Install App**
2. Select the repository where you want to install it
3. Click **Install**

## Step 3: Generate Private Key

1. In your GitHub App settings, scroll to **Private keys**
2. Click **Generate a private key**
3. **Save the private key file** - you'll need it in the next step
4. The key will be downloaded as a `.pem` file

## Step 4: Get App ID

1. In your GitHub App settings, find the **App ID** (it's displayed at the top)
2. Copy this number - you'll need it in the next step

## Step 5: Add Secrets to Repository

1. Go to your repository → **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**

   **Add first secret:**
   - **Name**: `DHWANI_RELEASE_BOT_APP_ID`
   - **Value**: The App ID from Step 4
   - Click **Add secret**

   **Add second secret:**
   - **Name**: `DHWANI_RELEASE_BOT_PRIVATE_KEY`
   - **Value**: Open the `.pem` file from Step 3, copy its entire contents (including `-----BEGIN RSA PRIVATE KEY-----` and `-----END RSA PRIVATE KEY-----`)
   - Click **Add secret**

## Step 6: Configure Branch Protection (Optional but Recommended)

To allow the bot to bypass branch protection:

1. Go to **Settings** → **Branches** → **Branch protection rules**
2. Edit the rule for `main`/`master`
3. Under **Restrict who can push to matching branches**, add:
   - `dhwani-release-bot[bot]`
4. Or under **Allow specified actors to bypass required pull requests**, add:
   - `dhwani-release-bot[bot]`

## Step 7: Verify Setup

1. Push a commit to `main` or `master` with a semantic commit message (e.g., `feat: add new feature`)
2. Check the **Actions** tab - the release workflow should run automatically
3. If successful, a new release should be created

## Using the Bot

### Automatic Releases

The bot automatically creates releases when:
- Code is pushed to `main` or `master` branches
- Commits follow semantic commit conventions (e.g., `feat:`, `fix:`, `BREAKING CHANGE:`)

### Manual Release Trigger

You can manually trigger a release by commenting in an issue or PR:

```
@dhwani-release-bot release
```

or

```
@dhwani-release-bot trigger-release
```

### Get Help

To see available commands:

```
@dhwani-release-bot help
```

## Developer Workflow

Developers can commit normally - the bot only handles releases:

```bash
# Normal developer workflow
git checkout -b feat/my-feature
git commit -m "feat: add new feature"
git push origin feat/my-feature
# Create PR, merge to main
# Bot automatically creates release on merge to main
```

## Troubleshooting

### Bot can't push to protected branch

- Ensure the GitHub App has the correct permissions
- Check that `dhwani-release-bot[bot]` is added to branch protection bypass list
- Verify the secrets are correctly set

### Release not triggered

- Check that commits follow semantic commit format
- Verify the workflow file is in `.github/workflows/release.yml`
- Check the Actions tab for workflow run logs

### Bot commands not working

- Ensure the bot handler workflow (`.github/workflows/bot-handler.yml`) exists
- Check that the GitHub App is installed on the repository
- Verify secrets are correctly configured

## Security Notes

- The private key is sensitive - never commit it to the repository
- Use repository secrets to store credentials
- Regularly rotate the private key if needed
- The bot only has permissions you grant it during setup

## Support

For issues or questions, please open an issue in the repository.

