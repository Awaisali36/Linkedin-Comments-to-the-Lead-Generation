# 🚀 LinkedIn Comments to Leads Extractor

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![n8n](https://img.shields.io/badge/n8n-Automation-orange)](https://n8n.io/)
[![Apify](https://img.shields.io/badge/Apify-Scraping-blue)](https://apify.com/)
[![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Export-green)](https://sheets.google.com/)

> **Transform LinkedIn post engagement into qualified leads automatically. Extract, enrich, and export commenters' profiles with full data in minutes.**

Turn every LinkedIn post into a lead generation machine. This intelligent automation identifies engaged prospects from post comments and delivers enriched profiles ready for outreach.



## 🎬 Demo Video

### See the Full System in Action

<div align="center">

  <a href="https://youtu.be/SNPm8o_b-vo">
    <img src="https://img.youtube.com/vi/SNPm8o_b-vo/maxresdefault.jpg" alt="Facebook Ad Manager - Complete Demo" width="100%">
  </a>

  <br><br>

  <a href="https://www.youtube.com/watch?v=Z_zoyLifS1s">
    <strong>▶️ Watch Full Demo on YouTube (5 minutes)</strong>
  </a>

</div>


---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture)
- [Prerequisites](#-prerequisites)
- [Installation & Setup](#-installation--setup)
- [Usage Guide](#-usage-guide)
- [Demo Video](#-demo-video)
- [Output Data Fields](#-output-data-fields)
- [API Configuration](#-api-configuration)
- [Troubleshooting](#-troubleshooting)
- [Use Cases](#-use-cases)
- [Contributing](#-contributing)
- [License](#-license)



## 🎯 Overview

**LinkedIn Comments to Leads Extractor** is a fully automated lead generation system that converts LinkedIn post engagement into actionable sales prospects. Simply provide LinkedIn post URLs, and the system extracts all commenters, enriches their profiles with complete data, and exports everything to Google Sheets.

### Who Is This For?

- 🎯 **Sales Teams** - Find warm leads already engaging with your content
- 📈 **Marketing Agencies** - Build targeted prospect lists from competitor posts
- 💼 **B2B Businesses** - Identify decision-makers engaging with industry content
- 🚀 **Growth Hackers** - Scale outreach with qualified, engaged prospects
- 👥 **Recruiters** - Find candidates commenting on relevant posts

### The Problem It Solves

Traditional LinkedIn lead generation requires:
- Manually copying commenter profiles one by one
- Visiting each profile to collect contact information
- Organizing data in spreadsheets manually
- **Hours of repetitive work per post**

This system automates everything in **minutes**, extracting and enriching hundreds of profiles automatically.

---

## ✨ Key Features

### 🤖 **Fully Automated Workflow**
- Zero manual copying or data entry
- Intelligent pagination for posts with 100+ comments
- Automatic profile deduplication
- Batch processing for optimal performance

### 🔍 **Deep Profile Enrichment**
- Full name, headline, and location
- Current company and position
- Profile picture and background image
- Complete work experience history
- Education and certifications
- Follower and connection counts
- Creator/influencer/premium status

### 📊 **Smart Data Extraction**
- Handles multiple post formats (URLs, IDs, activity links)
- Configurable comment limit per post
- Extracts unique profiles automatically
- Filters out duplicate commenters across posts

### 🎯 **Flexible Input Options**
- Direct LinkedIn post URLs
- Post activity IDs
- Numeric post identifiers
- Batch processing of multiple posts

### 📈 **Export to Google Sheets**
- Automatically creates dated spreadsheet
- Organized lead data in clean format
- Ready for CRM import or outreach campaigns
- Instant access to results

### ⚡ **Enterprise-Grade Scraping**
- Powered by Apify's LinkedIn scrapers
- No cookies or login required
- Handles rate limiting automatically
- Scalable to thousands of profiles

---

## 🛠️ Tech Stack

| Category | Technology | Purpose |
|----------|------------|---------|
| **Automation** | n8n | Workflow orchestration and logic |
| **Scraping** | Apify | LinkedIn data extraction |
| | - Comments Scraper | Extract post comments and authors |
| | - Profile Scraper | Enrich profile data |
| **Export** | Google Sheets | Lead list storage and sharing |
| **Triggers** | n8n Form | User-friendly web interface |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INPUT (FORM)                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  • LinkedIn Post URLs/IDs                                       │
│  • Number of comments to scrape (default: 100)                  │
│                                                                  │
└───────────────────────┬─────────────────────────────────────────┘
                        │
                        ↓
┌─────────────────────────────────────────────────────────────────┐
│                     N8N WORKFLOW PROCESSING                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Step 1: Parse Input                                            │
│  └─→ Clean URLs/IDs, set scrape limits                          │
│                                                                  │
│  Step 2: Scrape Comments (Apify)                                │
│  └─→ Extract all comments from post(s)                          │
│  └─→ Handle pagination (100 per page)                           │
│  └─→ Gather all commenter profile URLs                          │
│                                                                  │
│  Step 3: Deduplicate Profiles                                   │
│  └─→ Create unique list of commenters                           │
│  └─→ Remove duplicate profiles                                  │
│                                                                  │
│  Step 4: Batch Processing                                       │
│  └─→ Split profiles into batches of 500                         │
│  └─→ Optimize for Apify API limits                              │
│                                                                  │
│  Step 5: Profile Enrichment (Apify)                             │
│  └─→ Fetch full LinkedIn profile data                           │
│  └─→ Extract all available fields                               │
│  └─→ Aggregate results                                          │
│                                                                  │
│  Step 6: Data Preparation                                       │
│  └─→ Flatten nested objects                                     │
│  └─→ Format for Google Sheets                                   │
│                                                                  │
│  Step 7: Export to Google Sheets                                │
│  └─→ Create new spreadsheet                                     │
│  └─→ Name: "LinkedIn-Posts-Comments-Leads-[DATE]"               │
│  └─→ Populate with all lead data                                │
│                                                                  │
└───────────────────────┬─────────────────────────────────────────┘
                        │
                        ↓
┌─────────────────────────────────────────────────────────────────┐
│                      GOOGLE SHEETS OUTPUT                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Enriched Lead List with 28+ Data Fields                        │
│  • Personal Info  • Company Data  • Experience                  │
│  • Education     • Location       • Social Stats                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 Prerequisites

Before installation, ensure you have:

### Required Accounts & API Keys

- ✅ **n8n Instance** - Self-hosted or n8n Cloud
- ✅ **Apify Account** - With API token (free tier available)
- ✅ **Google Account** - For Google Sheets export
- ✅ **Google Sheets API** - OAuth credentials configured in n8n

### Apify Actors Needed

This workflow uses two Apify actors:
1. **LinkedIn Post Comments Scraper** (`apimaestro~linkedin-post-comments-replies-engagements-scraper-no-cookies`)
2. **LinkedIn Profile Batch Scraper** (`apimaestro~linkedin-profile-batch-scraper-no-cookies-required`)

Both are available on Apify Marketplace.

---

## 🚀 Installation & Setup

### Step 1: Clone or Download the Workflow

Download the `LinkedIn Comments to Leads Extractor.json` file from this repository.

### Step 2: Import into n8n

1. Open your n8n instance
2. Click **"Workflows"** → **"Import from File"**
3. Select the downloaded JSON file
4. Click **"Import"**

### Step 3: Configure API Credentials

#### 3.1 Set Apify API Token

1. Get your Apify API token from https://console.apify.com/account/integrations
2. In the workflow, find the **"Set APIFY Token"** node
3. Replace the empty `APIFY_TOKEN` value with your token:
   ```javascript
   "APIFY_TOKEN": "your_apify_api_token_here"
   ```

#### 3.2 Configure Google Sheets

1. In n8n, go to **Settings** → **Credentials**
2. Create new **"Google Sheets OAuth2 API"** credential
3. Follow the OAuth flow to authorize n8n
4. The workflow nodes are already configured to use Google Sheets

### Step 4: Activate the Workflow

1. Click **"Active"** toggle in top-right corner
2. Your workflow is now live and ready to use!

---

## 📖 Usage Guide

### Method 1: Using the Web Form (Recommended)

#### Step 1: Access the Form

1. Open your workflow in n8n
2. Find the **"On form submission"** node
3. Click on it to see the **Production URL**
4. Copy and share this URL (it's your lead generation form)

#### Step 2: Fill the Form

Navigate to the form URL and enter:

**Post IDs/URLs** (textarea):
```
7334385757769416704
https://www.linkedin.com/posts/username_title-activity-7334385757769416704-dMnz
https://www.linkedin.com/feed/update/urn:li:activity:7318557377971023872/
```

**How many comments to scrape** (number):
```
100
```
*Default is 100 if left empty*

#### Step 3: Submit and Wait

1. Click **"Submit"**
2. The workflow begins processing immediately
3. You'll be automatically redirected to Google Sheets when complete
4. Find your results in: **"LinkedIn-Posts-Comments-Leads-[TODAY'S DATE]"**

---

### Supported LinkedIn Post Formats

The system accepts multiple LinkedIn URL formats:

#### 1️⃣ **Numeric Post ID**
```
7334385757769416704
```

#### 2️⃣ **Activity URL**
```
https://www.linkedin.com/feed/update/urn:li:activity:7334385757769416704/
```

#### 3️⃣ **Full Post URL**
```
https://www.linkedin.com/posts/username_title-activity-7334385757769416704-dMnz
```

**All formats work!** The system automatically extracts the post ID.

---

### Method 2: Manual Trigger (Testing)

For testing purposes, you can use the manual trigger:

1. Enable the **"Trigger manually"** node (currently disabled)
2. Update the **"Set manual fields"** node with test URLs
3. Click **"Execute Workflow"**
4. View results in Google Sheets

---

## 🎬 Demo Video

### Watch the Full Walkthrough

<!-- Add your YouTube video here -->
[![LinkedIn Leads Extractor Demo](https://img.youtube.com/vi/YOUR_VIDEO_ID/maxresdefault.jpg)](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)

**[▶️ Watch Full Demo (5 minutes)](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)**

### What You'll See:

- **0:00** - Introduction & Use Case
- **0:45** - Submitting the form with LinkedIn posts
- **1:30** - Workflow processing in n8n
- **2:15** - Profile enrichment in action
- **3:00** - Google Sheets export
- **4:30** - Using the data for outreach

---

## 📊 Output Data Fields

Your Google Sheets export includes **28+ enriched data fields**:

### Basic Information
- `profileUrl` - Direct link to LinkedIn profile
- `basic_info_fullname` - Full name
- `basic_info_first_name` - First name
- `basic_info_last_name` - Last name
- `basic_info_headline` - Professional headline
- `basic_info_public_identifier` - LinkedIn username
- `basic_info_about` - Profile summary/bio

### Visual Assets
- `basic_info_profile_picture_url` - Profile photo URL
- `basic_info_background_picture_url` - Banner image URL

### Location Data
- `basic_info_location_country` - Country
- `basic_info_location_city` - City
- `basic_info_location_full` - Full location string
- `basic_info_location_country_code` - ISO country code

### Company Information
- `basic_info_current_company` - Current employer name
- `basic_info_current_company_url` - Company LinkedIn URL
- `basic_info_current_company_urn` - Company identifier

### Social Metrics
- `basic_info_follower_count` - Number of followers
- `basic_info_connection_count` - Connection count
- `basic_info_show_follower_count` - Public follower display setting

### Profile Status
- `basic_info_is_creator` - LinkedIn creator status
- `basic_info_is_influencer` - Influencer badge
- `basic_info_is_premium` - Premium subscription status
- `basic_info_creator_hashtags` - Featured hashtags

### Professional History
- `experience` - Full work experience (JSON array)
- `education` - Educational background (JSON array)
- `certifications` - Professional certifications (JSON array)

### Metadata
- `basic_info_created_timestamp` - Profile creation date
- `basic_info_urn` - LinkedIn URN identifier

---

## 🔐 API Configuration

### Apify Authentication

The workflow uses Apify's REST API with token-based authentication:

```javascript
// In "Set APIFY Token" node
{
  "APIFY_TOKEN": "your_apify_api_token_here"
}
```

**API Endpoints Used:**

1. **Comments Scraper**
```
POST https://api.apify.com/v2/acts/apimaestro~linkedin-post-comments-replies-engagements-scraper-no-cookies/run-sync-get-dataset-items?token=YOUR_TOKEN
```

2. **Profile Scraper**
```
POST https://api.apify.com/v2/acts/apimaestro~linkedin-profile-batch-scraper-no-cookies-required/run-sync-get-dataset-items?token=YOUR_TOKEN
```

---

### Google Sheets Authentication

Uses OAuth2 flow configured in n8n credentials manager:

1. Go to **n8n Settings** → **Credentials**
2. Add **"Google Sheets OAuth2 API"**
3. Follow authorization flow
4. Credential is referenced in workflow automatically

---

## 🐛 Troubleshooting

### Common Issues

#### Form Submission Fails

**Problem**: Form doesn't trigger workflow

**Solution**:
- Verify workflow is **Active** (toggle in top-right)
- Check webhook URL is accessible
- Test with manual trigger first
- Review n8n execution logs

---

#### No Comments Extracted

**Problem**: Apify returns empty results

**Solution**:
- Verify LinkedIn post URLs are public
- Check Apify API token is valid
- Ensure post has comments (try with known post)
- Verify post ID format is correct

---

#### Profile Enrichment Fails

**Problem**: Google Sheets has incomplete profile data

**Solution**:
- Check Apify account has sufficient credits
- Verify profile URLs are valid
- Review batch processing (should be 500 per batch)
- Check for rate limiting in Apify dashboard

---

#### Google Sheets Export Error

**Problem**: "Could not create spreadsheet" error

**Solution**:
- Re-authenticate Google Sheets credentials in n8n
- Verify Google Drive has storage space
- Check OAuth scopes include Sheets creation
- Test with manual Google Sheets node creation

---

#### Pagination Not Working

**Problem**: Only getting first 100 comments when post has more

**Solution**:
- Check **"More runs needed?"** conditional logic
- Verify `limit` parameter is > 100
- Review loop counter in **"Set pagination"** node
- Check Apify actor supports pagination

---

## 💡 Use Cases

### 1. **Sales Prospecting**
Extract engaged leads from your company's LinkedIn posts. These are warm prospects already familiar with your brand.

### 2. **Competitor Analysis**
Scrape comments from competitor posts to identify their engaged audience and build targeted outreach lists.

### 3. **Influencer Outreach**
Find engaged followers of industry influencers by extracting commenters from their viral posts.

### 4. **Event Marketing**
Collect attendee lists from LinkedIn event posts and announcements for follow-up campaigns.

### 5. **Community Building**
Identify active community members from industry thought leaders' posts to invite to your own community.

### 6. **Recruitment**
Find candidates engaging with job postings or industry content to build talent pipelines.

### 7. **Content Collaboration**
Identify creators and influencers engaging with specific topics for potential partnerships.

### 8. **Market Research**
Analyze who's commenting on industry trends to understand your target audience better.

---

## 🎯 Best Practices

### ✅ Do's

- **Target engaging posts** - Posts with 100+ comments yield better leads
- **Use recent posts** - Fresh engagement = warmer leads
- **Batch multiple posts** - Process 5-10 posts at once for efficiency
- **Export regularly** - Create dated sheets for campaign tracking
- **Verify data quality** - Spot-check first few results before scaling

### ⚠️ Considerations

- **Respect privacy** - Use data ethically for legitimate business purposes
- **LinkedIn ToS** - Automated scraping may violate LinkedIn's terms of service
- **Rate limits** - Apify has usage quotas; monitor your consumption
- **Data freshness** - Profile data reflects scrape time; may become outdated
- **GDPR compliance** - If targeting EU profiles, ensure GDPR compliance

---

## 🤝 Contributing

We welcome contributions to improve this workflow!

### How to Contribute

1. **Report Bugs** - Open an issue with detailed reproduction steps
2. **Suggest Features** - Share ideas for enhancements
3. **Submit Improvements** - Fork, modify, and submit pull requests
4. **Share Use Cases** - Document how you're using the workflow

### Development Guidelines

- Test changes with manual trigger before submitting
- Document any new nodes or logic clearly
- Maintain compatibility with existing Apify actors
- Update README with any new features

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

### What This Means

✅ Commercial use allowed  
✅ Modification allowed  
✅ Distribution allowed  
✅ Private use allowed  
⚠️ No warranty or liability

---

## 💬 Support

### Get Help

- 📧 **Email**: support@yourdomain.com
- 💬 **Discord**: [Join our community](https://discord.gg/yourserver)
- 🐛 **GitHub Issues**: [Report bugs](https://github.com/yourusername/linkedin-leads-extractor/issues)
- 📖 **Documentation**: [Full docs](https://docs.yourdomain.com)

---

## 🌟 Acknowledgments

Built with powerful tools:

- [n8n](https://n8n.io) - Workflow automation platform
- [Apify](https://apify.com) - Web scraping and automation
- [Google Sheets](https://sheets.google.com) - Data export and sharing

Special thanks to:
- Apify's **apimaestro** for LinkedIn scraping actors
- n8n community for workflow inspiration

---

## 🚀 Roadmap

### Planned Features

- [ ] Email finding integration (Hunter.io, Apollo.io)
- [ ] CRM direct export (HubSpot, Salesforce, Pipedrive)
- [ ] AI-powered lead scoring
- [ ] Automated outreach sequences
- [ ] Slack/Discord notifications
- [ ] Multi-language support
- [ ] Custom field mapping
- [ ] Duplicate detection across campaigns
- [ ] Lead tracking dashboard
- [ ] Export to CSV/Excel formats

---

## 📈 Performance Stats

**Average Processing Time:**
- 100 comments: ~2-3 minutes
- 500 comments: ~8-10 minutes
- 1000 comments: ~15-18 minutes

**Batch Efficiency:**
- Profiles per batch: 500
- Concurrent processing: Yes
- Deduplication: Automatic

**Export Speed:**
- Google Sheets: <30 seconds
- Ready for immediate use

---

**Made with ❤️ for Sales and Marketing Teams**

⭐ Star this repo if it helps your lead generation!

📢 Share with teams who need automated prospecting!

🔗 Fork and customize for your workflow!

---

**Need custom modifications or enterprise support?**  
Contact us at: letsautomatewithawais@gmail.com
