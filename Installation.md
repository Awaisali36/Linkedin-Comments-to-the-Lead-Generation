# 📦 Installation Guide

Complete step-by-step installation guide for LinkedIn Comments to Leads Extractor.

---

## 📋 Table of Contents

- [Prerequisites](#-prerequisites)
- [Step 1: n8n Setup](#-step-1-n8n-setup)
- [Step 2: Apify Account Setup](#-step-2-apify-account-setup)
- [Step 3: Google Account Setup](#-step-3-google-account-setup)
- [Step 4: Import Workflow](#-step-4-import-workflow)
- [Step 5: Configure Credentials](#-step-5-configure-credentials)
- [Step 6: Test the Workflow](#-step-6-test-the-workflow)
- [Step 7: Activate & Deploy](#-step-7-activate--deploy)
- [Post-Installation](#-post-installation)
- [Troubleshooting Installation](#-troubleshooting-installation)

---

## ✅ Prerequisites

Before beginning installation, ensure you have:

### Required Accounts

| Service | Required? | Purpose | Cost |
|---------|-----------|---------|------|
| **n8n** | ✅ Yes | Workflow automation platform | Free (self-hosted) or $20/mo (cloud) |
| **Apify** | ✅ Yes | LinkedIn scraping service | Free tier: 5,000 credits/mo |
| **Google Account** | ✅ Yes | Google Sheets export | Free |

### Technical Requirements

- **For n8n Cloud:**
  - Active n8n cloud account
  - Web browser

- **For Self-Hosted n8n:**
  - Node.js 16.x or higher
  - 2GB RAM minimum
  - Linux/macOS/Windows server
  - OR Docker installed

### Time Required

- **Total Installation Time:** 20-30 minutes
- **n8n Setup:** 5-10 minutes
- **Apify Configuration:** 5 minutes
- **Google Sheets Setup:** 5 minutes
- **Workflow Import & Testing:** 5-10 minutes

---

## 🚀 Step 1: n8n Setup

Choose between n8n Cloud (easiest) or Self-Hosted (more control).

### Option A: n8n Cloud (Recommended for Beginners)

#### 1.1 Create Account

1. Visit https://n8n.io/
2. Click **"Get started for free"**
3. Sign up with email or Google
4. Choose a plan:
   - **Starter:** Free trial
   - **Pro:** $20/month (recommended)

#### 1.2 Access Your Instance

1. Log in to https://app.n8n.cloud/
2. Your workspace is ready!
3. Skip to [Step 2: Apify Account Setup](#-step-2-apify-account-setup)

---

### Option B: Self-Hosted n8n

Choose your preferred installation method:

#### Method 1: Docker (Easiest for Self-Hosting)

**Prerequisites:**
- Docker installed
- Docker Compose installed

**Steps:**

1. **Create directory:**
   ```bash
   mkdir n8n-data
   cd n8n-data
   ```

2. **Create docker-compose.yml:**
   ```yaml
   version: '3.8'

   services:
     n8n:
       image: n8nio/n8n
       restart: always
       ports:
         - "5678:5678"
       environment:
         - N8N_BASIC_AUTH_ACTIVE=true
         - N8N_BASIC_AUTH_USER=admin
         - N8N_BASIC_AUTH_PASSWORD=your_secure_password_here
         - N8N_HOST=localhost
         - N8N_PORT=5678
         - N8N_PROTOCOL=http
         - NODE_ENV=production
         - WEBHOOK_URL=http://localhost:5678/
       volumes:
         - ./data:/home/node/.n8n
   ```

3. **Start n8n:**
   ```bash
   docker-compose up -d
   ```

4. **Access n8n:**
   - Open browser: http://localhost:5678
   - Login with credentials from docker-compose.yml

---

#### Method 2: npm (Direct Installation)

**Prerequisites:**
- Node.js 16.x or higher installed

**Steps:**

1. **Install n8n globally:**
   ```bash
   npm install n8n -g
   ```

2. **Start n8n:**
   ```bash
   n8n start
   ```

3. **Access n8n:**
   - Open browser: http://localhost:5678
   - Create your first user account

---

#### Method 3: npx (Quick Test)

**For testing without installation:**

```bash
npx n8n
```

Access at: http://localhost:5678

**Note:** Data is not persisted with this method!

---

### Verify n8n Installation

1. Access your n8n instance (cloud or local)
2. You should see the n8n dashboard
3. Click **"Workflows"** in the sidebar
4. You're ready to proceed!

---

## 🔧 Step 2: Apify Account Setup

Apify provides the LinkedIn scraping capabilities.

### 2.1 Create Apify Account

1. Visit https://apify.com/
2. Click **"Sign up free"**
3. Create account with email or GitHub
4. Verify your email address

### 2.2 Get Free Credits

New accounts receive **5,000 free credits per month**!

**Credit Usage Estimate:**
- Comments Scraper: ~5-10 credits per 100 comments
- Profile Scraper: ~10-20 credits per 100 profiles
- **Total for 100 leads:** ~15-30 credits

With free tier, you can process **~1,500-3,000 leads per month!**

### 2.3 Get Your API Token

1. Log in to https://console.apify.com/
2. Go to **Settings** → **Integrations**
3. Copy your **API Token**
4. Save it securely (you'll need it in Step 5)

**Example Token Format:**
```
apify_api_aBcDeFgHiJkLmNoPqRsTuVwXyZ123456
```

### 2.4 Subscribe to Required Actors (Free)

You need two Apify actors:

#### Actor 1: Comments Scraper

1. Visit: https://apify.com/apimaestro/linkedin-post-comments-replies-engagements-scraper-no-cookies
2. Click **"Try for free"**
3. No subscription needed - pay-per-use with your credits

#### Actor 2: Profile Scraper

1. Visit: https://apify.com/apimaestro/linkedin-profile-batch-scraper-no-cookies-required
2. Click **"Try for free"**
3. No subscription needed - pay-per-use with your credits

### 2.5 Verify Apify Setup

**Quick Test:**

1. Go to one of the actors
2. Click **"Try it"**
3. Test with a sample LinkedIn URL
4. Verify it returns data
5. Check your credit usage in dashboard

✅ If the actor runs successfully, you're ready!

---

## 📊 Step 3: Google Account Setup

### 3.1 Requirements

- Active Google account
- Access to Google Drive
- Google Sheets enabled

### 3.2 Enable Google Sheets API (for Self-Hosted n8n)

**If using n8n Cloud, skip this step!**

For self-hosted n8n, you need to create OAuth credentials:

1. **Go to Google Cloud Console:**
   - Visit: https://console.cloud.google.com/

2. **Create a Project:**
   - Click "Select a project" → "New Project"
   - Name: "n8n LinkedIn Leads"
   - Click "Create"

3. **Enable Google Sheets API:**
   - Go to "APIs & Services" → "Library"
   - Search: "Google Sheets API"
   - Click "Enable"

4. **Create OAuth Credentials:**
   - Go to "APIs & Services" → "Credentials"
   - Click "Create Credentials" → "OAuth client ID"
   - Application type: "Web application"
   - Name: "n8n"
   - Authorized redirect URIs:
     ```
     http://localhost:5678/rest/oauth2-credential/callback
     ```
   - Click "Create"

5. **Save Credentials:**
   - Copy **Client ID**
   - Copy **Client Secret**
   - You'll need these in Step 5

### 3.3 Verify Google Setup

1. Open Google Sheets: https://sheets.google.com/
2. Create a test spreadsheet
3. Verify you can edit and save
4. Delete the test spreadsheet

✅ Google account is ready!

---

## 📥 Step 4: Import Workflow

### 4.1 Download Workflow File

1. Go to this repository
2. Download: **"LinkedIn Comments to Leads Extractor.json"**
3. Save to your computer

### 4.2 Import to n8n

#### Method 1: Import from File (Recommended)

1. **Open n8n**
2. **Click "Workflows" in sidebar**
3. **Click "Import from File" button** (top-right)
4. **Select the downloaded JSON file**
5. **Click "Import"**

#### Method 2: Import from URL

1. **Click "Workflows" → "Import from URL"**
2. **Paste raw GitHub URL:**
   ```
   https://raw.githubusercontent.com/yourusername/linkedin-leads-extractor/main/LinkedIn%20Comments%20to%20Leads%20Extractor.json
   ```
3. **Click "Import"**

#### Method 3: Copy-Paste JSON

1. **Open the JSON file in text editor**
2. **Copy all content (Ctrl+A, Ctrl+C)**
3. **In n8n: Workflows → Import from File**
4. **Click "Paste JSON" tab**
5. **Paste and click "Import"**

### 4.3 Verify Import

After import, you should see:

- **Workflow Name:** "LinkedIn Comments to Leads Extractor"
- **Total Nodes:** 28 nodes
- **Trigger:** "On form submission" (webhook)
- **End Node:** "Done"

✅ Workflow imported successfully!

---

## 🔐 Step 5: Configure Credentials

Now configure all API connections.

### 5.1 Set Apify API Token

1. **Open the imported workflow**
2. **Find node: "Set APIFY Token"**
3. **Click on the node**
4. **Find the assignment: `APIFY_TOKEN`**
5. **Replace empty value with your Apify token:**
   ```javascript
   {
     "id": "20febbcc-0a59-4a89-8932-9bbb34b0e9f9",
     "name": "APIFY_TOKEN",
     "type": "string",
     "value": "apify_api_YOUR_TOKEN_HERE"  // ← Replace this
   }
   ```
6. **Click "Execute Node"** to test
7. **Verify no errors**

---

### 5.2 Configure Google Sheets OAuth

#### For n8n Cloud:

1. **Find any Google Sheets node** (e.g., "Create Google Sheet")
2. **Click on the node**
3. **In "Credential to connect with" dropdown:**
   - Click **"Create New Credential"**
4. **Select: "Google Sheets OAuth2 API"**
5. **Click "Connect my account"**
6. **Authorize n8n** in popup window
7. **Credential is saved automatically**

#### For Self-Hosted n8n:

1. **Go to n8n Settings** (gear icon)
2. **Click "Credentials"**
3. **Click "Add Credential"**
4. **Select "Google Sheets OAuth2 API"**
5. **Enter your OAuth credentials from Step 3.2:**
   - Client ID: `[your-client-id]`
   - Client Secret: `[your-client-secret]`
6. **Click "Connect my account"**
7. **Complete OAuth flow**
8. **Test connection**

---

### 5.3 Verify All Credentials

Check these nodes have credentials:

- ✅ **"Set APIFY Token"** - Has your Apify token
- ✅ **"Create Google Sheet"** - Has Google OAuth credential
- ✅ **"Add Leads"** - Has Google OAuth credential

**Test Credentials:**

1. Click on "Create Google Sheet" node
2. Click "Execute Node"
3. If successful, a test sheet is created
4. Check Google Drive for the new sheet
5. Delete the test sheet

✅ All credentials configured!

---

## 🧪 Step 6: Test the Workflow

### 6.1 Enable Manual Trigger (for testing)

The workflow has a manual trigger disabled by default. Let's enable it:

1. **Find node: "Trigger manually"**
2. **Right-click on the node**
3. **Select "Enable"**
4. **Find node: "Set manual fields"**
5. **Right-click → Enable**

### 6.2 Configure Test Data

1. **Click on "Set manual fields" node**
2. **Update with test LinkedIn post URLs:**
   ```javascript
   {
     "assignments": {
       "assignments": [
         {
           "name": "postIds",
           "type": "array",
           "value": [
             "https://www.linkedin.com/posts/yourtest-activity-123456789/",
             "7334385757769416704"
           ]
         },
         {
           "name": "limit",
           "type": "number",
           "value": 100
         }
       ]
     }
   }
   ```

### 6.3 Run Test Execution

1. **Make sure you're viewing the workflow canvas**
2. **Click "Execute Workflow" button** (play icon, top-right)
3. **Watch the execution flow:**
   - Green = Success
   - Red = Error
   - Gray = Skipped

### 6.4 Monitor Execution

Watch the progress:

1. **Step 1:** Parse input (should be instant)
2. **Step 2:** Scrape comments (~30-60 seconds)
3. **Step 3:** Deduplicate profiles (instant)
4. **Step 4:** Enrich profiles (~2-3 minutes)
5. **Step 5:** Export to Google Sheets (~10 seconds)

### 6.5 Verify Results

1. **Check Google Drive**
2. **Look for sheet:** "LinkedIn-Posts-Comments-Leads-[DATE]"
3. **Open the sheet**
4. **Verify data columns:**
   - profileUrl
   - basic_info_fullname
   - basic_info_headline
   - basic_info_current_company
   - etc. (28+ fields total)

✅ If you see enriched data, the workflow works!

---

## ✅ Step 7: Activate & Deploy

### 7.1 Disable Test Nodes

1. **Find "Trigger manually" node**
2. **Right-click → Disable**
3. **Find "Set manual fields" node**
4. **Right-click → Disable**

### 7.2 Activate Workflow

1. **Toggle "Active" switch** (top-right corner)
2. **Switch should turn green**
3. **Status changes to "Active"**

### 7.3 Get Form URL

1. **Click on "On form submission" node**
2. **Copy the "Production URL"**
3. **Example:**
   ```
   https://your-n8n-instance.app.n8n.cloud/form/a81d04b5-169a-4368-b925-91fb0660b6b2
   ```

### 7.4 Test Production Form

1. **Open the Production URL in new tab**
2. **You should see the form:**
   - Title: "Linkedin Posts Comments Leads Scraper"
   - Two fields: Post IDs/URLs and Comment limit
3. **Fill with test data:**
   - Post URLs: Paste a real LinkedIn post URL
   - Comment limit: 50 (for faster testing)
4. **Click "Submit"**
5. **You'll be redirected to Google Sheets**
6. **Check for your results**

✅ Production form is working!

---

## 🎉 Post-Installation

### Share Your Form

Now that everything works, share the form URL with your team:

```
https://your-n8n-instance.app.n8n.cloud/form/[your-unique-id]
```

**Use cases:**
- Add to company wiki/knowledge base
- Share in Slack/Teams
- Bookmark for quick access
- Add to sales team resources

---

### Set Up Monitoring (Optional)

#### Email Notifications on Error:

1. Add "Send Email" node after any critical nodes
2. Configure to send only on error
3. Route error outputs to notification

#### Slack Notifications:

1. Add "Slack" node
2. Send message when workflow completes
3. Include link to Google Sheet

---

### Optimize for Your Use Case

#### Adjust Comment Limits:

- **Quick leads (50-100):** Faster processing
- **Deep research (500-1000):** More comprehensive

#### Customize Output Fields:

1. Edit "Prepare the list for export" node
2. Add/remove fields as needed
3. Modify Google Sheets columns

---

### Create Workflow Backups

1. **Export workflow regularly:**
   - Workflows → Click menu (⋮) → Export
2. **Save JSON to version control (Git)**
3. **Store in secure backup location**

---

## 🐛 Troubleshooting Installation

### Issue: Can't Import Workflow

**Error:** "Invalid JSON format"

**Solution:**
1. Ensure you downloaded the complete file
2. Check file isn't corrupted
3. Try copy-paste method instead
4. Verify JSON is valid: https://jsonlint.com/

---

### Issue: Apify Token Not Working

**Error:** "401 Unauthorized"

**Solution:**
1. Verify token is correct (no extra spaces)
2. Check token in Apify dashboard is active
3. Regenerate token if needed
4. Ensure quotes around token in workflow

---

### Issue: Google Sheets Authorization Fails

**Error:** "OAuth error" or "Access denied"

**Solution:**
1. Check popup wasn't blocked
2. Allow popups for n8n domain
3. Try incognito/private window
4. Re-authenticate from scratch
5. For self-hosted: verify redirect URI matches

---

### Issue: Test Execution Fails

**Error:** Various errors during test

**Solution:**
1. **Check each node individually:**
   - Click node → "Execute Node"
2. **Verify credentials are set:**
   - Look for credential warnings (yellow icon)
3. **Check Apify credits:**
   - You need available credits
4. **Verify LinkedIn URLs are public:**
   - Private posts won't work

---

### Issue: No Data in Google Sheets

**Possible causes:**

1. **LinkedIn post has no comments**
   - Solution: Test with post that has comments
   
2. **Profile scraping failed**
   - Solution: Check Apify dashboard for errors
   
3. **Google Sheets node didn't execute**
   - Solution: Check execution log for errors

---

### Issue: Workflow Too Slow

**Solution:**

1. **Reduce comment limit** - Test with 50 instead of 100
2. **Check Apify performance** - View run logs in Apify
3. **Optimize batch sizes** - Currently set to 500
4. **Check internet speed** - Slow connection affects performance

---

### Need More Help?

- 📧 **Email:** support@yourdomain.com
- 💬 **GitHub Issues:** [Report a bug](https://github.com/yourusername/linkedin-leads-extractor/issues)
- 📖 **Documentation:** See [CONFIGURATION.md](CONFIGURATION.md) and [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- 🎥 **Video Tutorial:** [Watch installation guide](https://youtube.com/...)

---

## ✅ Installation Complete!

**Congratulations!** Your LinkedIn Comments to Leads Extractor is now fully installed and ready to use.

### What's Next?

1. ✅ Read [CONFIGURATION.md](CONFIGURATION.md) for advanced settings
2. ✅ Check [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for common issues
3. ✅ Start generating leads from LinkedIn posts!
4. ✅ Share feedback or contribute improvements

---

**Estimated setup time:** You should have completed installation in 20-30 minutes.

**Ready to extract leads?** Open your form URL and start processing LinkedIn posts! 🚀
