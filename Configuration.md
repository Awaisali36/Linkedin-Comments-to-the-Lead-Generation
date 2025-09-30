# ⚙️ Configuration Guide

Advanced configuration options for LinkedIn Comments to Leads Extractor.

---

## 📋 Table of Contents

- [Apify Configuration](#-apify-configuration)
- [Google Sheets Configuration](#-google-sheets-configuration)
- [Workflow Settings](#-workflow-settings)
- [Form Customization](#-form-customization)
- [Output Customization](#-output-customization)
- [Performance Tuning](#-performance-tuning)
- [Notification Setup](#-notification-setup)
- [Security Settings](#-security-settings)
- [Advanced Options](#-advanced-options)

---

## 🔧 Apify Configuration

### API Token Setup

#### Location in Workflow
Node: **"Set APIFY Token"**

#### Configuration
```javascript
{
  "assignments": {
    "assignments": [
      {
        "id": "20febbcc-0a59-4a89-8932-9bbb34b0e9f9",
        "name": "APIFY_TOKEN",
        "type": "string",
        "value": "apify_api_YOUR_TOKEN_HERE"  // ← Your token
      }
    ]
  }
}
```

#### Getting Your Token

1. **Login to Apify:** https://console.apify.com/
2. **Navigate to:** Settings → Integrations
3. **Copy API Token**
4. **Format:** `apify_api_xxxxxxxxxxxxxxxxxx`

#### Token Security Best Practices

✅ **Do:**
- Store token in n8n credentials manager (recommended)
- Use environment variables for production
- Rotate tokens periodically
- Restrict token permissions if possible

❌ **Don't:**
- Commit tokens to version control
- Share tokens in documentation
- Use same token across environments
- Leave tokens in plain text

---

### Apify Actor Configuration

#### Actor 1: Comments Scraper

**Node:** "Run Apify Comments Scraper"

**Configuration:**
```javascript
{
  "method": "POST",
  "url": "https://api.apify.com/v2/acts/apimaestro~linkedin-post-comments-replies-engagements-scraper-no-cookies/run-sync-get-dataset-items?token={{ $json.APIFY_TOKEN }}",
  "bodyParameters": {
    "postIds": ["..."],      // Array of post URLs
    "page_number": 1,         // Current page
    "sortOrder": "most recent", // Or "most relevant"
    "limit": 100              // Comments per post
  }
}
```

**Parameters Explained:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `postIds` | Array | Required | LinkedIn post URLs or IDs |
| `page_number` | Number | 1 | Pagination page number |
| `sortOrder` | String | "most recent" | Sort by "most recent" or "most relevant" |
| `limit` | Number | 100 | Max comments to fetch |

**Customization Options:**

1. **Change Sort Order:**
   ```javascript
   "sortOrder": "most relevant"  // For quality over recency
   ```

2. **Adjust Page Size:**
   ```javascript
   "limit": 50  // Faster processing, fewer comments
   ```

---

#### Actor 2: Profile Enrichment

**Node:** "Run Apify Profile Enrichment"

**Configuration:**
```javascript
{
  "method": "POST",
  "url": "https://api.apify.com/v2/acts/apimaestro~linkedin-profile-batch-scraper-no-cookies-required/run-sync-get-dataset-items?token={{ $json.APIFY_TOKEN }}",
  "bodyParameters": {
    "usernames": ["..."]  // Array of LinkedIn profile URLs
  }
}
```

**Batch Processing:**
- Default: 500 profiles per batch
- Configurable in "Split in batches" node
- Higher = faster but more API load

**To adjust batch size:**

1. Find node: **"Split in batches"**
2. Modify JavaScript code:
   ```javascript
   const chunkSize = 500;  // Change this value
   ```
3. Recommended: 100-1000

---

### Apify Credit Management

#### Understanding Credits

**Comments Scraper:**
- ~0.05-0.10 credits per comment
- ~5-10 credits per 100 comments

**Profile Scraper:**
- ~0.10-0.20 credits per profile
- ~10-20 credits per 100 profiles

**Total for 100 Leads:**
- Comments: ~5-10 credits
- Profiles: ~10-20 credits
- **Total: ~15-30 credits**

#### Monitoring Usage

1. **Apify Dashboard:** https://console.apify.com/
2. **View "Usage & Billing"**
3. **Check "Credits"** section
4. **Set alerts** for low balance

#### Free Tier Limits

- **5,000 credits/month** for free
- **~150-300 leads/month** on free tier
- Upgrade for more: $49/mo = 50,000 credits

---

## 📊 Google Sheets Configuration

### OAuth Setup

#### For n8n Cloud

**Auto-configured!** Just click "Connect my account" in any Google Sheets node.

---

#### For Self-Hosted n8n

**Prerequisites:**
- Google Cloud Project
- Google Sheets API enabled
- OAuth 2.0 credentials created

**Configuration Steps:**

1. **In n8n Settings:**
   - Go to **Credentials**
   - Add **"Google Sheets OAuth2 API"**

2. **Enter OAuth Details:**
   ```
   Client ID: [your-client-id]
   Client Secret: [your-client-secret]
   ```

3. **Redirect URI:**
   ```
   http://localhost:5678/rest/oauth2-credential/callback
   ```
   Or for production:
   ```
   https://your-domain.com/rest/oauth2-credential/callback
   ```

4. **Authorize:**
   - Click "Connect my account"
   - Complete OAuth flow
   - Grant permissions

---

### Sheet Creation Settings

**Node:** "Create Google Sheet"

**Configuration:**
```javascript
{
  "resource": "spreadsheet",
  "title": "LinkedIn-Posts-Comments-Leads-{{ $now.format('yyyy-MM-dd+T') }}",
  "sheetsUi": {
    "sheetValues": [
      {
        "title": "Leads"  // Sheet tab name
      }
    ]
  }
}
```

**Customization Options:**

#### 1. Change Sheet Naming Convention

```javascript
// Date-based (default)
"title": "LinkedIn-Leads-{{ $now.format('yyyy-MM-dd') }}"

// Include time
"title": "Leads-{{ $now.format('yyyy-MM-dd-HH-mm') }}"

// Include post identifier
"title": "Leads-{{ $json.postId }}-{{ $now.format('yyyy-MM-dd') }}"

// Custom prefix
"title": "ClientName-Leads-{{ $now.format('yyyy-MM-dd') }}"
```

#### 2. Add Multiple Sheets

```javascript
"sheetsUi": {
  "sheetValues": [
    { "title": "Leads" },
    { "title": "Summary" },
    { "title": "Notes" }
  ]
}
```

#### 3. Set Default Folder

Currently creates in root Drive. To organize:

1. **After creation**, move to folder manually
2. **Or** use Google Drive API to set parent folder
3. **Or** use Google Drive node to move file

---

### Data Export Settings

**Node:** "Add Leads"

**Configuration:**
```javascript
{
  "operation": "append",
  "documentId": "{{ $('Create Google Sheet').first().json.spreadsheetId }}",
  "sheetName": "{{ $('Create Google Sheet').first().json.sheets[0].properties.sheetId }}",
  "columns": {
    "mappingMode": "autoMapInputData",
    "attemptToConvertTypes": false,
    "convertFieldsToString": false
  },
  "options": {
    "cellFormat": "USER_ENTERED",
    "useAppend": true
  }
}
```

**Options Explained:**

| Option | Value | Purpose |
|--------|-------|---------|
| `cellFormat` | "USER_ENTERED" | Allows formulas and auto-formatting |
| | "RAW" | Plain text only |
| `useAppend` | true | Add to end (recommended) |
| | false | Insert at specific position |
| `attemptToConvertTypes` | false | Keep as strings |
| | true | Convert to numbers/dates |

---

## 🔄 Workflow Settings

### Pagination Configuration

**Node:** "More runs needed?"

Controls when to fetch additional pages of comments.

**Current Logic:**
```javascript
// Fetch more pages if:
1. Post has > 100 total comments
2. User requested > 100 comments
```

**Customization:**

```javascript
// In "More runs needed?" node
{
  "conditions": [
    {
      "leftValue": "{{ $json.summary.totalComments }}",
      "operator": "gt",
      "rightValue": 100  // Change this threshold
    }
  ]
}
```

**Examples:**

```javascript
// Always get all comments (no limit)
"rightValue": 0

// Only paginate if > 500 comments
"rightValue": 500

// Never paginate (first 100 only)
// Disable the entire pagination branch
```

---

### Deduplication Settings

**Node:** "Create Unique List of Leads"

**Current Logic:**
```javascript
// Remove duplicate profiles based on profile_url
const uniqueProfiles = items.reduce((acc, item) => {
  if (item.author && !acc.some(i => i.profile_url === item.author.profile_url)) {
    acc.push(item.author);
  }
  return acc;
}, []);
```

**Customization Options:**

#### 1. Deduplicate by Different Field

```javascript
// By email (if available)
if (!acc.some(i => i.email === item.author.email)) { ... }

// By name
if (!acc.some(i => i.fullname === item.author.fullname)) { ... }

// By company
if (!acc.some(i => i.company === item.author.company)) { ... }
```

#### 2. Keep Duplicates (Don't Deduplicate)

```javascript
// Simply return all authors
const allProfiles = items.map(item => item.author);
return allProfiles;
```

#### 3. Track Duplicate Count

```javascript
// Add counter for how many times profile appears
const profileCounts = {};
items.forEach(item => {
  const url = item.author.profile_url;
  profileCounts[url] = (profileCounts[url] || 0) + 1;
});
```

---

### Batch Processing

**Node:** "Split in batches"

**Configuration:**
```javascript
const chunkSize = 500;  // Profiles per batch
```

**Tuning Guidelines:**

| Batch Size | Use Case | Pros | Cons |
|------------|----------|------|------|
| 100 | Testing | Fast, safe | More API calls |
| 500 | Default | Balanced | - |
| 1000 | High volume | Fewer API calls | Slower per batch |
| 2000+ | Enterprise | Maximum efficiency | Risk of timeout |

**To change:**

1. Find node: **"Split in batches"**
2. Edit JavaScript:
   ```javascript
   const chunkSize = 1000;  // Your preferred size
   ```
3. Test with smaller dataset first

---

## 📝 Form Customization

### Form Fields

**Node:** "On form submission"

**Current Configuration:**
```javascript
{
  "formFields": {
    "values": [
      {
        "fieldLabel": "Post IDs/URLs",
        "fieldType": "textarea",
        "placeholder": "e.g. 7334385757769416704, https://...",
        "requiredField": true
      },
      {
        "fieldLabel": "How many comments you want to scrape?",
        "fieldType": "number",
        "placeholder": "Default: 100"
      }
    ]
  }
}
```

---

### Adding Custom Fields

#### Example 1: Add Campaign Name Field

```javascript
{
  "fieldLabel": "Campaign Name",
  "fieldType": "text",
  "placeholder": "e.g. Q1-2025-Outreach",
  "requiredField": false
}
```

Then in workflow:
```javascript
// In "Set fields from the form" node
{
  "name": "campaignName",
  "value": "={{ $json['Campaign Name'] }}"
}
```

#### Example 2: Add Dropdown for Sort Order

```javascript
{
  "fieldLabel": "Sort Comments By",
  "fieldType": "dropdown",
  "fieldOptions": {
    "values": [
      { "option": "Most Recent" },
      { "option": "Most Relevant" }
    ]
  },
  "requiredField": false
}
```

#### Example 3: Add Checkbox for Options

```javascript
{
  "fieldLabel": "Include Profile Pictures",
  "fieldType": "checkbox",
  "requiredField": false
}
```

---

### Form Styling

**Current Configuration:**
```javascript
{
  "formTitle": "Linkedin Posts Comments Leads Scraper",
  "formDescription": "<div style=\"font-family: Arial...\">
```

**Customization:**

#### Change Colors

```html
<div style="color: #0077b5;">  <!-- LinkedIn blue -->
<div style="background: #f5f5f5; padding: 15px;">  <!-- Gray box -->
```

#### Add Logo

```html
<img src="https://your-domain.com/logo.png" style="width: 150px; margin-bottom: 20px;">
```

#### Modify Layout

```html
<!-- Two-column layout -->
<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
  <div>Column 1</div>
  <div>Column 2</div>
</div>
```

---

### Form Redirect

**After submission**, user is redirected:

**Current:**
```javascript
{
  "respondWithOptions": {
    "values": {
      "respondWith": "redirect",
      "redirectUrl": "https://docs.google.com/spreadsheets/u/0/"
    }
  }
}
```

**Alternative Options:**

#### 1. Redirect to Specific Sheet
```javascript
"redirectUrl": "={{ $('Create Google Sheet').first().json.spreadsheetUrl }}"
```

#### 2. Show Success Message Instead
```javascript
{
  "respondWith": "text",
  "responseText": "✅ Success! Check your email for the Google Sheet link."
}
```

#### 3. Redirect to Custom Page
```javascript
"redirectUrl": "https://your-website.com/thank-you?status=success"
```

---

## 📤 Output Customization

### Selecting Fields to Export

**Node:** "Prepare the list for export"

**Current:** Exports ALL fields

**To customize:**

```javascript
// Only export specific fields
return $input.first().json.items.map(item => ({
  json: {
    // Basic fields only
    profileUrl: item.profileUrl,
    fullname: item.basic_info?.fullname,
    headline: item.basic_info?.headline,
    location: item.basic_info?.location?.full,
    company: item.basic_info?.current_company,
    
    // Add calculated fields
    profile_strength: item.basic_info?.follower_count > 1000 ? "High" : "Low"
  }
}));
```

---

### Field Transformations

#### Example: Format Names

```javascript
// Capitalize names
fullname: item.basic_info?.fullname?.toUpperCase()

// Split name
firstName: item.basic_info?.first_name,
lastName: item.basic_info?.last_name
```

#### Example: Clean Data

```javascript
// Remove null values
Object.fromEntries(
  Object.entries(flatten(item)).filter(([_, v]) => v != null)
)
```

#### Example: Add Computed Fields

```javascript
// Add engagement score
engagement_score: (item.basic_info?.follower_count || 0) * 0.1 + 
                  (item.basic_info?.connection_count || 0) * 0.05
```

---

### Export Format Options

#### Option 1: Current (Google Sheets)

**Pros:** Easy sharing, collaborative, real-time  
**Cons:** Needs Google account

---

#### Option 2: Add CSV Export

**Add after "Prepare the list for export" node:**

1. **Add "Convert to File" node**
2. **Configure:**
   ```javascript
   {
     "fileFormat": "csv",
     "fileName": "leads-{{ $now.format('yyyy-MM-dd') }}.csv",
     "options": {
       "headerRow": true
     }
   }
   ```
3. **Add file storage node** (Dropbox, S3, etc.)

---

#### Option 3: Add Email Delivery

**Add "Send Email" node:**

```javascript
{
  "to": "your-email@company.com",
  "subject": "LinkedIn Leads - {{ $now.format('yyyy-MM-dd') }}",
  "text": "Attached: {{ $json.items.length }} leads",
  "attachments": "={{ $binary }}"  // Attach CSV
}
```

---

## ⚡ Performance Tuning

### Optimization Settings

#### 1. Reduce API Calls

```javascript
// In "Set APIFY Token" node
{
  "limit": 50  // Fetch fewer comments (faster)
}
```

#### 2. Parallel Processing

**Enable in n8n settings:**
- Workflows → Settings → Execution
- Set "Max execution parallelism": 5

#### 3. Timeout Settings

```javascript
// In HTTP Request nodes
{
  "options": {
    "timeout": 30000  // 30 seconds (increase if needed)
  }
}
```

#### 4. Batch Size Optimization

```javascript
// Smaller batches = more frequent updates
const chunkSize = 250;

// Larger batches = fewer API calls
const chunkSize = 1000;
```

---

### Memory Management

**For large datasets (1000+ profiles):**

1. **Increase n8n memory:**
   ```bash
   # Docker
   environment:
     - NODE_OPTIONS=--max-old-space-size=4096
   ```

2. **Process in chunks:**
   - Already implemented via batch processing
   - Adjust batch size as needed

3. **Clear intermediate data:**
   - Use "Remove Other Fields" node
   - Keep only necessary data between steps

---

## 🔔 Notification Setup

### Email Notifications

**Add after "Done" node:**

```javascript
{
  "node": "Send Email",
  "parameters": {
    "to": "team@company.com",
    "subject": "✅ Leads Ready: {{ $('Prepare the list for export').item.json.items.length }} profiles",
    "text": "Google Sheet: {{ $('Create Google Sheet').first().json.spreadsheetUrl }}"
  }
}
```

---

### Slack Notifications

**Add Slack node:**

```javascript
{
  "channel": "#leads",
  "text": "🎯 New leads extracted!\n\n*Count:* {{ $json.items.length }}\n*Sheet:* {{ $json.sheetUrl }}\n\n_Generated by LinkedIn Leads Extractor_"
}
```

---

### Discord Webhook

```javascript
{
  "method": "POST",
  "url": "YOUR_DISCORD_WEBHOOK_URL",
  "body": {
    "content": "✅ LinkedIn leads ready!",
    "embeds": [{
      "title": "Lead Export Complete",
      "description": "{{ $json.items.length }} profiles extracted",
      "url": "{{ $json.sheetUrl }}",
      "color": 65280
    }]
  }
}
```

---

## 🔒 Security Settings

### API Key Protection

**Best practices:**

1. **Use n8n Credentials Manager**
   - Store Apify token as credential
   - Reference as `{{ $credentials.apify.apiKey }}`

2. **Environment Variables**
   ```bash
   # .env file
   APIFY_TOKEN=apify_api_xxxxxxxxx
   ```

3. **Restrict Webhook Access**
   - Add basic auth to form
   - Use n8n's built-in authentication

---

### Rate Limiting

**Protect against abuse:**

1. **Add rate limit node** (custom function)
2. **Track requests per IP**
3. **Limit: 10 requests/hour per IP**

```javascript
// Pseudo-code
const requestLog = {};
const ip = $input.first().json.headers['x-forwarded-for'];
const count = requestLog[ip] || 0;

if (count > 10) {
  throw new Error('Rate limit exceeded');
}
```

---

### Data Privacy

**GDPR Compliance:**

1. **Add privacy notice** to form
2. **Store minimal data only**
3. **Auto-delete old sheets** (optional)
4. **Provide data export/deletion** mechanism

---

## 🚀 Advanced Options

### Multi-Post Processing

**Process multiple posts efficiently:**

Already supported! Just add multiple URLs:
```
https://linkedin.com/posts/post1
https://linkedin.com/posts/post2
https://linkedin.com/posts/post3
```

---

### Custom Filtering

**Add filtering logic:**

```javascript
// In "Create Unique List of Leads" node
const filteredProfiles = uniqueProfiles.filter(profile => {
  // Only profiles with 500+ connections
  return profile.connection_count > 500;
  
  // Only profiles in specific locations
  return profile.location?.country === 'United States';
  
  // Only premium users
  return profile.is_premium === true;
});
```

---

### CRM Integration

**Add after Google Sheets export:**

#### HubSpot Example:

```javascript
{
  "node": "HubSpot",
  "operation": "create",
  "resource": "contact",
  "properties": {
    "email": "{{ $json.email }}",
    "firstname": "{{ $json.first_name }}",
    "lastname": "{{ $json.last_name }}",
    "company": "{{ $json.current_company }}",
    "linkedin_url": "{{ $json.profileUrl }}"
  }
}
```

---

### Schedule Automatic Runs

**Use Cron trigger:**

```javascript
{
  "node": "Cron",
  "cronExpression": "0 9 * * 1"  // Every Monday at 9 AM
}
```

Then process predefined post list automatically.

---

## 📚 Configuration Files

### Create .env Template

```env
# Apify Configuration
APIFY_API_TOKEN=your_token_here

# Google Sheets
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_secret

# n8n Configuration
N8N_WEBHOOK_URL=https://your-n8n-instance.com

# Optional: Notifications
SLACK_WEBHOOK=
EMAIL_RECIPIENT=

# Optional: Performance
BATCH_SIZE=500
MAX_COMMENTS=100
TIMEOUT=30000
```

---

## 🎯 Configuration Best Practices

### ✅ Do's

- **Test changes** with small datasets first
- **Document customizations** in code comments
- **Version control** your workflow JSON
- **Backup configurations** before major changes
- **Use descriptive node names** for clarity

### ❌ Don'ts

- **Don't hardcode** sensitive data
- **Don't skip testing** after changes
- **Don't modify** core logic without understanding
- **Don't forget** to update documentation
- **Don't ignore** error handling

---

## 💡 Need Help?

For advanced configuration assistance:

- 📧 **Email:** support@yourdomain.com
- 💬 **Discord:** [Join community](https://discord.gg/yourserver)
- 📖 **Docs:** [Full documentation](https://docs.yourdomain.com)

---

**Configuration complete!** Your workflow is now customized to your needs. 🚀
