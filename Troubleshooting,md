# 🔧 Troubleshooting Guide

Complete troubleshooting guide for LinkedIn Comments to Leads Extractor.

---

## 📋 Table of Contents

- [Quick Diagnostics](#-quick-diagnostics)
- [Common Issues](#-common-issues)
- [Form Submission Issues](#-form-submission-issues)
- [Apify Scraping Issues](#-apify-scraping-issues)
- [Data Processing Issues](#-data-processing-issues)
- [Google Sheets Issues](#-google-sheets-issues)
- [Performance Issues](#-performance-issues)
- [Error Messages](#-error-messages)
- [Debugging Tips](#-debugging-tips)
- [Getting Help](#-getting-help)

---

## 🩺 Quick Diagnostics

### Check Workflow Health

Run this checklist before troubleshooting:

- [ ] **Workflow is Active** (green toggle in top-right)
- [ ] **All credentials are set** (no yellow warning icons)
- [ ] **Apify token is valid** (check Apify dashboard)
- [ ] **Google Sheets connected** (test with manual execution)
- [ ] **n8n has internet access** (can reach external APIs)
- [ ] **Form URL is accessible** (open in browser)

---

### Test Workflow Step-by-Step

1. **Open workflow in n8n**
2. **Enable manual trigger** (right-click "Trigger manually" → Enable)
3. **Click "Execute Node"** on each node individually
4. **Check for errors** (red indicators)
5. **Review output data** (green success indicators)

---

### Check Execution Logs

**In n8n:**
1. Go to **"Executions"** tab (left sidebar)
2. Find your latest run
3. Click to view detailed execution
4. Look for failed nodes (red)
5. Check error messages

**Useful filters:**
- Status: Error
- Status: Success
- Date range: Last 24 hours

---

## ⚠️ Common Issues

### Issue 1: "Workflow Not Responding"

**Symptoms:**
- Form submitted but nothing happens
- No execution appears in logs
- Redirect doesn't work

**Possible Causes & Solutions:**

#### Cause 1: Workflow Not Active

**Solution:**
```
1. Open workflow in n8n
2. Check "Active" toggle (top-right)
3. If OFF, click to turn ON (green)
4. Resubmit form
```

#### Cause 2: Webhook URL Changed

**Solution:**
```
1. Click "On form submission" node
2. Copy new "Production URL"
3. Update any bookmarks/links
4. Test with new URL
```

#### Cause 3: n8n Instance Down

**Solution:**
```
For self-hosted:
1. Check if n8n is running: `docker ps`
2. Restart if needed: `docker-compose restart`

For n8n Cloud:
1. Check n8n status page
2. Contact n8n support if down
```

---

### Issue 2: "No Data Extracted"

**Symptoms:**
- Workflow completes successfully
- Google Sheet is created but empty
- Or has headers only

**Possible Causes & Solutions:**

#### Cause 1: LinkedIn Post Has No Comments

**Solution:**
```
1. Check the LinkedIn post manually
2. Verify it has comments visible
3. Try with a post that definitely has 10+ comments
4. Test URLs:
   - Find a viral post (100+ comments)
   - Copy URL
   - Submit again
```

#### Cause 2: Post URL Format Invalid

**Solution:**
```
Valid formats:
✅ https://www.linkedin.com/posts/username_activity-1234567890/
✅ https://www.linkedin.com/feed/update/urn:li:activity:1234567890/
✅ 1234567890 (numeric ID only)

Invalid formats:
❌ https://linkedin.com/in/username (profile, not post)
❌ Shortened links (bit.ly, etc.)
❌ Post URL with extra parameters
```

**Fix:**
```
1. Open LinkedIn post in browser
2. Get clean URL from address bar
3. Remove tracking parameters (everything after '?')
4. Use that clean URL
```

#### Cause 3: Apify Scraper Failed Silently

**Solution:**
```
1. Check Apify dashboard: https://console.apify.com/
2. Go to "Runs" section
3. Find recent runs of both actors
4. Check for errors or empty results
5. If failing:
   - Verify post URL is public
   - Check you have credits remaining
   - Try different post URL
```

---

### Issue 3: "Partial Data / Missing Fields"

**Symptoms:**
- Some profiles have data, others don't
- Many fields show "null" or empty
- Inconsistent results

**Possible Causes & Solutions:**

#### Cause 1: LinkedIn Profiles Are Private

**Solution:**
```
Private profiles cannot be scraped. This is normal.

Expected behavior:
- Public profiles: Full data
- Private profiles: Limited data (name, headline only)

No fix available - this is by design.
```

#### Cause 2: Rate Limiting

**Solution:**
```
1. Check Apify dashboard for rate limit errors
2. Reduce batch size:
   - Find "Split in batches" node
   - Change chunkSize from 500 to 250
3. Add delay between batches:
   - Add "Wait" node after profile scraping
   - Set to 5-10 seconds
```

#### Cause 3: Network Timeout

**Solution:**
```
1. Increase timeout in HTTP Request nodes:

{
  "options": {
    "timeout": 60000  // 60 seconds (instead of 30)
  }
}

2. Check your internet connection
3. Try again during off-peak hours
```

---

## 📝 Form Submission Issues

### Error: "Form Not Found"

**Symptoms:**
- Form URL shows 404 error
- "Page not found" message

**Solution:**
```
1. Verify workflow is Active
2. Check webhook ID hasn't changed:
   - Click "On form submission" node
   - Copy current Production URL
3. If self-hosted, check n8n is running
4. Verify port 5678 is accessible (for self-hosted)
5. Check firewall settings
```

---

### Error: "Required Field Missing"

**Symptoms:**
- Form shows validation error
- Cannot submit even with data entered

**Solution:**
```
1. Ensure "Post IDs/URLs" field is not empty
2. Check for invisible characters (copy from notepad)
3. Verify field format:
   - Must contain at least one URL or ID
   - Separate multiple with commas or newlines
4. Clear browser cache and retry
```

---

### Error: "Redirect Failed"

**Symptoms:**
- Form submits but doesn't redirect
- Stays on form page or shows error

**Solution:**
```
1. Check browser popup blocker
2. Allow popups for n8n domain
3. Try different browser (Chrome, Firefox)
4. Check form redirect URL in workflow:
   - Should be: https://docs.google.com/spreadsheets/u/0/
   - Or: Specific sheet URL
```

---

### Issue: "Form Takes Forever to Submit"

**Symptoms:**
- Form spinning/loading indefinitely
- No response after clicking Submit

**Possible Causes:**

#### Cause 1: Large Dataset Processing

**Solution:**
```
Expected processing times:
- 50 comments: 2-3 minutes
- 100 comments: 3-5 minutes
- 500 comments: 10-15 minutes

Be patient! The workflow is running in background.
```

#### Cause 2: n8n Workflow Timeout

**Solution:**
```
For large datasets (500+ comments):

1. In n8n workflow settings:
   - Set "Execution timeout": 3600 (1 hour)
   
2. Or process in smaller batches:
   - Limit to 100 comments per run
   - Run multiple times for large posts
```

---

## 🔍 Apify Scraping Issues

### Error: "401 Unauthorized"

**Symptoms:**
- HTTP Request to Apify fails
- Error message: "Invalid token"

**Solution:**
```
1. Verify Apify token in "Set APIFY Token" node
2. Check token format: apify_api_xxxxxxxxxxxxxx
3. Regenerate token in Apify dashboard:
   - Settings → Integrations
   - "Regenerate token"
   - Update in n8n workflow
4. Ensure no extra spaces before/after token
```

---

### Error: "Actor Not Found"

**Symptoms:**
- Error: "Actor does not exist"
- 404 from Apify API

**Solution:**
```
Verify actor URLs in HTTP Request nodes:

Comments Scraper:
✅ apimaestro~linkedin-post-comments-replies-engagements-scraper-no-cookies

Profile Scraper:
✅ apimaestro~linkedin-profile-batch-scraper-no-cookies-required

If different:
1. Update URL in "Run Apify Comments Scraper" node
2. Update URL in "Run Apify Profile Enrichment" node
```

---

### Error: "Insufficient Credits"

**Symptoms:**
- Error message about credits
- Apify run fails immediately

**Solution:**
```
1. Check credit balance:
   - Apify Dashboard → Usage & Billing
   
2. Free tier: 5,000 credits/month
   - ~150-300 leads possible

3. If depleted:
   - Wait for monthly reset
   - Or upgrade plan ($49/mo = 50,000 credits)
   
4. Optimize usage:
   - Reduce comment limit
   - Process fewer posts per run
   - Avoid duplicate scraping
```

---

### Issue: "Scraper Returns Empty Results"

**Symptoms:**
- Apify runs successfully
- But returns empty array `[]`

**Possible Causes:**

#### Cause 1: LinkedIn Post Removed/Private

**Solution:**
```
1. Open post URL in browser
2. Check if accessible without login
3. If private/removed:
   - Post was deleted
   - Post was made private
   - Post URL is incorrect
4. Try different post URL
```

#### Cause 2: Rate Limited by LinkedIn

**Solution:**
```
LinkedIn may temporarily block Apify's scrapers.

Wait 24 hours and retry.

Or try during different times:
- Best: Late night / early morning (US time)
- Avoid: Peak business hours
```

#### Cause 3: Post ID Format Issue

**Solution:**
```
Extract post ID correctly:

From URL:
https://www.linkedin.com/posts/username_activity-7334385757769416704-dMnz

Post ID is: 7334385757769416704

Test extraction:
1. Find "Set fields from the form" node
2. Check postIds value in output
3. Should be clean numeric ID or full URL
```

---

## 🔄 Data Processing Issues

### Error: "Cannot Read Property of Undefined"

**Symptoms:**
- JavaScript error in Code nodes
- Workflow stops at "Create Unique List" or similar

**Solution:**
```
1. Check if previous node returned data:
   - Click previous node
   - Look at "Output" tab
   - Should see array of items

2. Add null checks in Code nodes:
   
// Before:
item.author.profile_url

// After:
item.author?.profile_url || ''

3. Verify data structure matches expectations
```

---

### Issue: "Duplicate Profiles Not Removed"

**Symptoms:**
- Same person appears multiple times
- Google Sheet has duplicate rows

**Solution:**
```
1. Check "Create Unique List of Leads" node
2. Verify logic:
   
const uniqueProfiles = items.reduce((acc, item) => {
  if (item.author && !acc.some(i => i.profile_url === item.author.profile_url)) {
    acc.push(item.author);
  }
  return acc;
}, []);

3. If still duplicates:
   - Person commented on multiple posts (expected)
   - Or profile_url field is null/different format
   
4. Add post-processing deduplication in Google Sheets
```

---

### Issue: "Flattening Error"

**Symptoms:**
- Error in "Prepare the list for export" node
- Cannot flatten nested objects

**Solution:**
```
1. Check flatten function exists
2. Test with single item first
3. Handle arrays properly:

// For arrays like experience/education
if (Array.isArray(value)) {
  res[prefixedKey] = JSON.stringify(value);  // Convert to string
}

4. Skip problematic fields if needed
```

---

## 📊 Google Sheets Issues

### Error: "Failed to Create Spreadsheet"

**Symptoms:**
- Error at "Create Google Sheet" node
- "Insufficient permissions" or similar

**Solution:**
```
1. Re-authenticate Google Sheets:
   - n8n Settings → Credentials
   - Find Google Sheets credential
   - Click "Reconnect"
   - Complete OAuth flow

2. Verify permissions granted:
   - Read/write access to Google Sheets
   - Create new spreadsheets
   - Access Google Drive

3. Check Google Drive storage:
   - Must have space available
   - Free tier: 15 GB
```

---

### Error: "Sheet Not Found" on Append

**Symptoms:**
- Sheet created successfully
- But "Add Leads" node fails

**Solution:**
```
1. Check sheet was actually created:
   - Look in Google Drive
   - Verify name matches pattern

2. Verify sheet ID passing correctly:
   
documentId: {{ $('Create Google Sheet').first().json.spreadsheetId }}
sheetName: {{ $('Create Google Sheet').first().json.sheets[0].properties.sheetId }}

3. Add delay between create and append:
   - Add "Wait" node: 2 seconds
   - Allows Google to process creation
```

---

### Issue: "Data Not Formatted Correctly"

**Symptoms:**
- All data in one column
- Fields not aligned
- Missing headers

**Solution:**
```
1. Check "columns" mapping in "Add Leads" node:
   
{
  "mappingMode": "autoMapInputData"  // Should auto-detect
}

2. Verify data structure before export:
   - Click "Set the list for sheets" node
   - Check output format
   - Should be array of flat objects

3. Test with manual column mapping if auto fails:
   
{
  "mappingMode": "defineBelow",
  "value": {
    "profileUrl": "={{ $json.profileUrl }}",
    "fullname": "={{ $json.basic_info_fullname }}"
    // ... more fields
  }
}
```

---

### Issue: "Sheet Has Wrong Name"

**Symptoms:**
- Sheet created but name is unexpected
- Can't find sheet by name

**Solution:**
```
Check naming in "Create Google Sheet" node:

Default:
"title": "LinkedIn-Posts-Comments-Leads-{{ $now.format('yyyy-MM-dd+T') }}"

Date format examples:
- yyyy-MM-dd = 2025-01-15
- yyyy-MM-dd+T = 2025-01-15T
- yyyyMMdd = 20250115

Customize as needed.
```

---

## ⚡ Performance Issues

### Issue: "Workflow Very Slow"

**Symptoms:**
- Takes 20+ minutes for 100 leads
- Times out frequently

**Diagnostics:**

1. **Check each stage timing:**
   - Comments scraping: Should be 1-2 min
   - Profile enrichment: Should be 2-3 min per 100
   - Export: Should be < 30 sec

2. **Identify bottleneck:**
   - Click each node
   - Check "Execution time" in output

**Solutions:**

#### For Slow Comment Scraping:

```
1. Reduce comment limit:
   - Change limit from 100 to 50
   
2. Check Apify performance:
   - View run in Apify dashboard
   - Check for delays/errors
```

#### For Slow Profile Enrichment:

```
1. Reduce batch size:
   - Change from 500 to 250 in "Split in batches"
   
2. Add parallel processing:
   - Currently sequential
   - Can't parallelize due to Apify rate limits
   
3. Filter profiles before enrichment:
   - Only enrich profiles with 500+ connections
   - Skip profiles without company info
```

#### For Network Issues:

```
1. Check n8n instance location:
   - Should be geographically close to you
   - Consider moving to closer region

2. Check Apify status:
   - https://status.apify.com/

3. Try different time:
   - Peak hours = slower
   - Try early morning/late night
```

---

### Issue: "Memory Errors"

**Symptoms:**
- "JavaScript heap out of memory"
- Workflow crashes mid-execution

**Solution:**

#### For Self-Hosted n8n:

```bash
# Docker - edit docker-compose.yml
environment:
  - NODE_OPTIONS=--max-old-space-size=4096  # 4GB RAM

# Or npm/npx
NODE_OPTIONS=--max-old-space-size=4096 n8n start
```

#### For n8n Cloud:

```
1. Contact n8n support for memory increase
2. Or optimize workflow:
   - Process in smaller batches
   - Clear intermediate data
   - Reduce profile fields extracted
```

#### Reduce Memory Usage:

```javascript
// In "Prepare the list for export"
// Only keep essential fields

return $input.first().json.items.map(item => ({
  json: {
    profileUrl: item.profileUrl,
    fullname: item.basic_info?.fullname,
    headline: item.basic_info?.headline,
    // Remove less important fields
  }
}));
```

---

## 🚨 Error Messages

### Error: "ECONNREFUSED"

**Meaning:** Cannot connect to external service

**Common Causes:**
- Apify API is down
- n8n has no internet access
- Firewall blocking requests

**Solution:**
```
1. Check internet connection
2. Verify n8n can reach internet:
   - Test with simple HTTP Request to google.com
3. Check firewall/proxy settings
4. Verify Apify status: https://status.apify.com/
```

---

### Error: "ETIMEDOUT"

**Meaning:** Request took too long

**Solution:**
```
1. Increase timeout in HTTP Request nodes:
   
{
  "options": {
    "timeout": 60000  // 60 seconds
  }
}

2. Check network speed
3. Try during off-peak hours
4. Reduce batch size for faster processing
```

---

### Error: "Invalid JSON"

**Meaning:** Response is not valid JSON

**Solution:**
```
1. Check Apify response format:
   - Should be JSON array
   - Might be HTML error page

2. Add error handling:
   
try {
  const data = JSON.parse(response);
} catch (e) {
  console.log('Raw response:', response);
  throw new Error('Invalid JSON response');
}

3. Check Apify actor is working:
   - Test in Apify console directly
   - Verify it returns JSON
```

---

### Error: "Workflow Execution Timed Out"

**Meaning:** Workflow took longer than timeout limit

**Solution:**
```
1. In workflow settings:
   - Increase "Execution timeout" to 3600 (1 hour)

2. Or split into smaller chunks:
   - Process 50 comments at a time
   - Run multiple times for large posts

3. Optimize workflow:
   - Remove unnecessary nodes
   - Reduce data processing
```

---

## 🐛 Debugging Tips

### Enable Debug Mode

```
1. In n8n Settings:
   - Enable "Show debug information"
   - Enable "Save execution data"

2. In workflow:
   - Add "Set" nodes to inspect data at each stage
   - Use console.log() in Code nodes

3. Check execution logs carefully:
   - Look at input/output of each node
   - Verify data structure
```

---

### Test Individual Nodes

```
1. Click any node
2. Click "Execute Node" (instead of whole workflow)
3. Check output in bottom panel
4. Verify data looks correct
5. Continue to next node
```

---

### Use Static Test Data

```
1. Disable form trigger
2. Enable manual trigger
3. Use "Set manual fields" with known-good data
4. Test full workflow with controlled input
5. Re-enable form when working
```

---

### Add Logging

```javascript
// In Code nodes, add logging:

console.log('Input data:', $input.all());
console.log('Processing items:', items.length);
console.log('Unique profiles:', uniqueProfiles.length);

// View logs in execution details
```

---

### Check Each Integration Separately

**Test Apify alone:**
```
1. Go to Apify console
2. Run actor manually with your post URL
3. Verify it returns data
4. Check credit usage
```

**Test Google Sheets alone:**
```
1. Create test workflow with just Google Sheets nodes
2. Try creating sheet
3. Try appending data
4. Verify permissions
```

---

## 📞 Getting Help

### Before Asking for Help

Collect this information:

1. **Error message** (exact text)
2. **Screenshot** of failed execution
3. **Node** where error occurred
4. **Input data** used (sanitize sensitive info)
5. **n8n version** (Settings → About)
6. **When it started** (worked before? recent changes?)

---

### Where to Get Help

#### Community Support (Free)

- **n8n Community Forum:** https://community.n8n.io/
- **GitHub Issues:** Report bugs in this repo
- **Discord:** Join our community server

#### Official Support

- **n8n Support:** support@n8n.io (for n8n Cloud users)
- **Apify Support:** support@apify.com (for Apify issues)

#### Paid Support

- **Consulting:** Custom implementation help
- **Priority Support:** Faster response times
- **Email:** support@yourdomain.com

---

### How to Report Issues

**Good issue report:**
```markdown
## Issue: Comments not extracting

**What I expected:**
Extract all 50 comments from post

**What happened:**
Only got 10 comments, workflow stopped

**Steps to reproduce:**
1. Submit form with URL: linkedin.com/posts/...
2. Wait for completion
3. Check Google Sheet - only 10 rows

**Environment:**
- n8n version: 1.xx.x
- n8n Cloud
- Apify free tier

**Error message:**
"Maximum retries exceeded"

**Screenshot:**
[attach image]
```

---

## 🎯 Quick Fixes Checklist

When something isn't working:

- [ ] Restart n8n (if self-hosted)
- [ ] Deactivate and reactivate workflow
- [ ] Clear browser cache
- [ ] Re-authenticate Google Sheets
- [ ] Verify Apify token
- [ ] Check credits remaining
- [ ] Test with different LinkedIn post
- [ ] Try smaller dataset (50 comments)
- [ ] Check execution logs
- [ ] Review this troubleshooting guide

---

## 🔍 Advanced Debugging

### Export Execution Data

```
1. Go to failed execution
2. Click "Download" button
3. Saves JSON with all data
4. Share with support team for analysis
```

---

### Check Webhook Logs

```bash
# For self-hosted n8n with Docker
docker logs n8n_container_name

# Look for webhook requests
# Verify POST requests are arriving
```

---

### Test API Endpoints Directly

**Test Apify API:**
```bash
curl -X POST \
  "https://api.apify.com/v2/acts/apimaestro~linkedin-post-comments-replies-engagements-scraper-no-cookies/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "postIds": ["7334385757769416704"],
    "limit": 10
  }'
```

**Test Google Sheets API:**
```
Use Google's OAuth Playground:
https://developers.google.com/oauthplayground/
```

---

## ✅ Resolution Checklist

Once issue is fixed:

- [ ] Document what was wrong
- [ ] Test with multiple scenarios
- [ ] Update workflow version
- [ ] Export backup
- [ ] Share solution with team
- [ ] Consider preventive measures
- [ ] Update documentation if needed

---

## 💡 Prevention Tips

### Avoid Common Issues

1. **Regular maintenance:**
   - Update n8n monthly
   - Test workflow after updates
   - Backup configurations

2. **Monitor resources:**
   - Watch Apify credit usage
   - Check Google Drive storage
   - Monitor n8n performance

3. **Best practices:**
   - Test with small datasets first
   - Use version control
   - Document customizations
   - Keep credentials secure

---

**Still having issues?** Don't hesitate to reach out for help! We're here to assist. 🚀

📧 **Email:** support@yourdomain.com  
💬 **Discord:** [Join our community](https://discord.gg/yourserver)  
🐛 **GitHub:** [Report an issue](https://github.com/yourusername/linkedin-leads-extractor/issues)
