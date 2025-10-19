# GitHub App Setup Guide

This guide will walk you through creating and configuring a GitHub App for the repository automation workflow.

## Why Use a GitHub App?

GitHub Apps provide several advantages over Personal Access Tokens (PATs):

- ✅ **Fine-grained permissions**: Only grant the specific permissions needed
- ✅ **Better security**: Credentials are scoped to the app, not a user account
- ✅ **Audit trail**: All actions are attributed to the app, making it clear they're automated
- ✅ **No user dependency**: Doesn't rely on a specific user's account staying active
- ✅ **Rate limits**: Higher API rate limits than PATs

## Step 1: Create a GitHub App

1. **Navigate to GitHub App settings**:

   **Option A - Organization (Recommended):**

   - Direct link: `https://github.com/organizations/RoboLancers/settings/apps/new`
   - Or navigate: Go to `https://github.com/RoboLancers` → Settings tab → GitHub Apps (in left sidebar) → New GitHub App
   - **Note**: You must be an organization **Owner** to see this option

   **Option B - Personal Account (Alternative):**

   - If you don't see GitHub Apps in your org settings, you can create it under your personal account
   - Go to: `https://github.com/settings/apps`
   - Click **"New GitHub App"**
   - You'll be able to install it on the organization later

2. **Configure Basic Information**:

   - **GitHub App name**: `RoboLancers Repository Manager` (or any unique name)
   - **Description**: `Automated repository configuration and management`
   - **Homepage URL**: `https://github.com/RoboLancers/management`
   - **Webhook**: Uncheck "Active" (we don't need webhooks)

3. **Set Permissions**:

   Under **Repository permissions**:

   - **Administration**: `Read and write` ⚠️ (Required for branch protection and repo settings)
   - **Contents**: `Read and write` (Required for creating files and pushing commits)
   - **Pull requests**: `Read and write` (Required for PR template)
   - **Workflows**: `Read and write` ⚠️ (Required for creating/updating workflow files)
   - **Metadata**: `Read-only` (Automatically selected)

   Under **Organization permissions** (if applicable):

   - **Members**: `Read-only` (Optional, for team-based restrictions)

4. **Where can this GitHub App be installed?**

   - Select **"Only on this account"** for organization-wide use
   - Or **"Any account"** if you want flexibility

5. **Click "Create GitHub App"**

## Step 2: Generate and Download Private Key

1. After creating the app, scroll down to **"Private keys"**
2. Click **"Generate a private key"**
3. A `.pem` file will be downloaded automatically
4. **⚠️ IMPORTANT**: Store this file securely - you cannot download it again!

## Step 3: Install the GitHub App

1. On the GitHub App page, click **"Install App"** in the left sidebar
2. Select **RoboLancers** organization (or your account)
3. Choose installation scope:

   - **All repositories**: Allows the app to manage all current and future repos
   - **Only select repositories**: Choose specific repos you want to manage

   Recommendation: Start with **"All repositories"** for full automation capability

4. Click **"Install"**

## Step 4: Get Your App ID

1. Go back to the GitHub App settings page
2. At the top, you'll see **"App ID"** - copy this number
3. Example: `123456`

## Step 5: Add Secrets to Repository

Now add the credentials to the `management` repository:

1. Go to `https://github.com/RoboLancers/management/settings/secrets/actions`

2. **Add APP_ID secret**:

   - Click **"New repository secret"**
   - Name: `APP_ID`
   - Value: Your App ID (e.g., `123456`)
   - Click **"Add secret"**

3. **Add APP_PRIVATE_KEY secret**:
   - Click **"New repository secret"**
   - Name: `APP_PRIVATE_KEY`
   - Value: Open the `.pem` file you downloaded and copy the **entire contents** including:
     ```
     -----BEGIN RSA PRIVATE KEY-----
     (all the encoded content)
     -----END RSA PRIVATE KEY-----
     ```
   - Click **"Add secret"**

## Step 6: Test the Setup

1. Go to the **Actions** tab in the management repository
2. Select the **"Setup Repository"** workflow
3. Click **"Run workflow"**
4. Enter a test repository name (e.g., `RoboLancers/test-repo`)
5. Click **"Run workflow"**

If everything is configured correctly:

- ✅ The workflow should complete successfully
- ✅ Branch protection rules will be applied to the target repository
- ✅ Repository settings will be updated
- ✅ PR template will be created

## Troubleshooting

### Cannot Find "GitHub Apps" in Organization Settings

**Possible Causes:**

1. **Not an Owner**: You need **Owner** role in the organization (not just Member or Admin)

   - Check your role: `https://github.com/orgs/RoboLancers/people`
   - Ask an existing owner to promote you

2. **Organization Settings Access**: Some organizations restrict settings access

   - Go to `https://github.com/organizations/RoboLancers/settings/member_privileges`
   - Check if third-party application access is disabled

3. **Wrong Navigation**: Make sure you're in the right place
   - ✅ Correct: Go to org Settings → Developer settings → GitHub Apps
   - ❌ Wrong: Organization home page or repository settings

**Solutions:**

- **Try the direct link**: `https://github.com/organizations/RoboLancers/settings/apps/new`
- **Use personal account**: Create the app under your personal account at `https://github.com/settings/apps` and install it on the org
- **Check browser**: Try incognito/private mode in case of cache issues
- **Contact GitHub Support**: If you're definitely an owner and still can't access it

### Error: "Resource not accessible by integration"

**Cause**: The GitHub App doesn't have sufficient permissions.

**Solution**:

1. Go to the GitHub App settings
2. Check that **Administration** permission is set to "Read and write"
3. If you changed permissions, you may need to accept the new permissions:
   - Go to the installed app in your organization
   - You should see a banner asking to accept new permissions
   - Click "Review request" and accept

### Error: "Bad credentials"

**Cause**: The private key or App ID is incorrect.

**Solution**:

1. Verify the `APP_ID` secret matches your App ID
2. Verify the `APP_PRIVATE_KEY` contains the complete `.pem` file contents
3. Make sure there are no extra spaces or line breaks when pasting

### Error: "App is not installed"

**Cause**: The GitHub App isn't installed for the target repository.

**Solution**:

1. Go to `https://github.com/organizations/RoboLancers/settings/installations`
2. Click on your app
3. Make sure the target repository is included in the installation

### Error: "Resource not found"

**Cause**: The repository name is incorrect or doesn't exist.

**Solution**:

1. Verify the repository exists
2. Check the format: `owner/repo` (e.g., `RoboLancers/robotics-2024`)
3. Ensure the GitHub App has access to that repository

## Security Best Practices

1. **Protect the Private Key**:

   - Never commit the `.pem` file to version control
   - Store it securely (password manager, encrypted storage)
   - Only store it in GitHub Secrets for the workflow

2. **Principle of Least Privilege**:

   - Only grant permissions the app actually needs
   - Review permissions periodically

3. **Audit App Activity**:

   - Check the audit log regularly: `https://github.com/organizations/RoboLancers/settings/audit-log`
   - Look for actions performed by the app

4. **Rotate Keys Periodically**:
   - Generate a new private key every 6-12 months
   - Delete old keys after rotation

## Managing the GitHub App

### View App Activity

```bash
# View recent activity by the app
gh api /orgs/RoboLancers/audit-log | jq '.[] | select(.actor == "RoboLancers Repository Manager")'
```

### Revoke Access

If you need to revoke the app's access:

1. Go to `https://github.com/organizations/RoboLancers/settings/installations`
2. Click on the app
3. Click **"Uninstall"**

### Update Permissions

1. Go to the GitHub App settings
2. Update the permissions under "Permissions & events"
3. Save changes
4. Go to the installation and accept the new permissions

## Additional Resources

- [GitHub Apps Documentation](https://docs.github.com/en/apps)
- [Creating a GitHub App](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/about-creating-github-apps)
- [Best practices for creating a GitHub App](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/best-practices-for-creating-github-apps)
- [actions/create-github-app-token](https://github.com/actions/create-github-app-token)
