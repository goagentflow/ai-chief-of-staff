# Office 365 Setup Skill

*Guides you through connecting Microsoft 365 (Outlook, Calendar, Teams, OneDrive) to your AI Chief of Staff.*

---

## When to Use This Skill

- During initial setup when user wants Office 365 integration
- When user says "Connect Office 365" or "Set up Microsoft integration"
- When user wants to access calendar, email, or Teams through Claude

---

## Overview

This skill helps you install and configure the [office-365-mcp-server](https://github.com/hvkshetry/office-365-mcp-server), which connects Claude to Microsoft 365 via the Graph API.

**What you'll get:**
- Read and send emails
- Manage calendar events
- Access Teams meetings and transcripts
- Work with OneDrive/SharePoint files
- Manage contacts and tasks

**Time required:** 10-15 minutes

---

## Setup Flow

### Step 1: Install the MCP Server

Say to the user:

"I'll download the Office 365 MCP server now. This connects Claude to your Microsoft 365 account."

**Run these commands:**

```bash
# Navigate to the ai-chief-of-staff directory
cd [path-to-ai-chief-of-staff]

# Clone the MCP server
git clone https://github.com/hvkshetry/office-365-mcp-server.git

# Install dependencies
cd office-365-mcp-server
npm install

# Create environment file
cp .env.example .env
```

---

### Step 2: Register an Azure App

Say to the user:

"Now you need to register an app in Azure. This is how Microsoft knows to trust the connection. I'll guide you through each step.

**Open this link:** [Azure App Registrations](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)

Then follow these steps:"

**Instructions to give:**

1. Click **"New registration"**

2. Fill in:
   - **Name:** `Claude Office MCP` (or whatever you prefer)
   - **Supported account types:** Select "Accounts in any organizational directory and personal Microsoft accounts"
   - **Redirect URI:**
     - Platform: `Web`
     - URI: `http://localhost:3000/auth/callback`

3. Click **Register**

4. On the overview page, copy the **Application (client) ID** - you'll need this

---

### Step 3: Add API Permissions

Say to the user:

"Now we need to grant the app permission to access your data. Still in the Azure portal:"

**Instructions to give:**

1. In the left sidebar, click **"API permissions"**

2. Click **"Add a permission"**

3. Select **"Microsoft Graph"**

4. Select **"Delegated permissions"**

5. Add these permissions (search for each one):

**Essential permissions:**
- `offline_access` - Keeps you logged in
- `User.Read` - Read your profile

**Email:**
- `Mail.Read` - Read your emails
- `Mail.ReadWrite` - Edit/delete emails
- `Mail.Send` - Send emails on your behalf

**Calendar:**
- `Calendars.Read` - View your calendar
- `Calendars.ReadWrite` - Create/edit events

**Files (OneDrive/SharePoint):**
- `Files.Read` - Read your files
- `Files.ReadWrite` - Edit your files

**Optional - Teams (add if you use Teams):**
- `Team.ReadBasic.All` - View Teams info
- `Chat.Read` - Read chat messages
- `OnlineMeetingTranscript.Read.All` - Access meeting transcripts

6. Click **"Add permissions"** after selecting all needed permissions

---

### Step 4: Create a Client Secret

Say to the user:

"Almost there. Now we need to create a secret key:"

**Instructions to give:**

1. In the left sidebar, click **"Certificates & secrets"**

2. Click **"New client secret"**

3. Add a description: `Claude MCP`

4. Set expiration: **24 months** (maximum)

5. Click **Add**

6. **IMPORTANT:** Copy the **Value** immediately - you won't see it again!

---

### Step 5: Configure the Environment

Say to the user:

"Now let's add your credentials to the config file."

**Help them edit the `.env` file:**

```bash
# Open the environment file
# Location: [path-to-ai-chief-of-staff]/office-365-mcp-server/.env
```

**Set these values:**

```
OFFICE_CLIENT_ID=<paste-your-application-client-id>
OFFICE_CLIENT_SECRET=<paste-your-secret-value>
OFFICE_TENANT_ID=common
OFFICE_REDIRECT_URI=http://localhost:3000/auth/callback
```

---

### Step 6: Authenticate

Say to the user:

"Final step - let's connect to your Microsoft account."

**Run:**

```bash
cd [path-to-ai-chief-of-staff]/office-365-mcp-server
npm run auth-server
```

**Then:**
1. Open http://localhost:3000/auth in your browser
2. Sign in with your Microsoft account
3. Approve the permissions
4. You'll see "Authentication successful" when done

---

### Step 7: Configure Claude Code

Say to the user:

"Now we need to tell Claude Code about this server."

**For Claude Code CLI, add to `~/.claude/settings.json`:**

```json
{
  "mcpServers": {
    "office-365": {
      "command": "node",
      "args": ["[path-to-ai-chief-of-staff]/office-365-mcp-server/index.js"],
      "env": {
        "OFFICE_CLIENT_ID": "your-client-id",
        "OFFICE_CLIENT_SECRET": "your-client-secret",
        "OFFICE_TENANT_ID": "common",
        "OFFICE_REDIRECT_URI": "http://localhost:3000/auth/callback"
      }
    }
  }
}
```

**Restart Claude Code** after making this change.

---

## Verification

After setup, test with:

- "What's on my calendar today?"
- "Show me my recent emails"
- "What meetings do I have this week?"

If these work, you're connected!

---

## Troubleshooting

### "Token expired" errors
Run `npm run auth-server` again and re-authenticate.

### "Permission denied" errors
Check that you added all required permissions in Azure and clicked "Add permissions".

### Server won't start
Make sure `.env` file has no extra spaces or quotes around values.

### Can't access Teams features
You need to add the Teams-related permissions (see Step 3) and re-authenticate.

---

## Security Notes

- Your credentials are stored locally - never share your `.env` file
- The client secret expires after 24 months - you'll need to create a new one
- You can revoke access anytime in Azure Portal > App registrations > Your app > Delete

---

## Updating the Server

To get the latest version:

```bash
cd [path-to-ai-chief-of-staff]/office-365-mcp-server
git pull
npm install
```

---

*This skill was created for the AI Chief of Staff system. For issues with the MCP server itself, see: https://github.com/hvkshetry/office-365-mcp-server*
