# 🚀 TUDEALS Pro Master Control v2.1
## Quick Start Guide - Ready for Production

### ✅ System Status: FULLY OPERATIONAL

---

## ⚡ What You Have

### 1. Two N8N Workflows
- **Facebook-AutoPost-PUBLISH-SCHEDULED v3.2** (14 nodes)
  - Detects missed/stale posts
  - Auto-requeue with +1 hour delay
  - Up to 10 retry attempts
  - Real-time lock detection

- **Facebook-Auto-Post-from-JOB_QUEUE v1.0** (16 nodes)
  - Reads from Google Sheets
  - Generates short links with Bitly
  - Validates product URLs
  - Posts to Facebook automatically

### 2. Google Sheets Integration
- JOB_QUEUE sheet (product queue)
- Performance sheet (results tracking)
- Real-time updates
- OAuth authentication

### 3. API Integrations
- 📤 Facebook Graph API
- 🔗 Google Sheets API
- 🔗 Bitly API
- 📄 All credentials configured

---

## 🚀 Getting Started in 5 Minutes

### Step 1: Deploy Workflows (2 min)
```bash
1. Open N8N Dashboard
2. Import JSON files:
   - Facebook-AutoPost-PUBLISH-SCHEDULED-v3.2__READY__ACTIVE.json
   - Facebook-Auto-Post-from-JOB_QUEUE-v1.0.json
3. Click "Activate" on both workflows
4. Verify status shows "Active"
```

### Step 2: Configure Google Sheets (2 min)
```bash
1. Create new Google Sheet or use existing
2. Create two tabs:
   - JOB_QUEUE (with columns: ID, product_name, price, image_url, shop_link, status, scheduled_time)
   - Performance (with columns: ID, post_id, platform, revenue, commission, timestamp)
3. Share sheet with N8N service account
4. Copy sheet ID to N8N credentials
```

### Step 3: Add First Product (1 min)
```bash
1. Open JOB_QUEUE sheet
2. Add product:
   - product_name: "iPhone 15 Pro"
   - price: 25000
   - image_url: "https://example.com/image.jpg"
   - shop_link: "https://shopee.co.th/product"
   - status: "APPROVED"
   - scheduled_time: "2025-12-29 18:00:00"
3. Save and wait
```

### Step 4: Test First Post (No time needed!)
```bash
1. Go to N8N Dashboard
2. Find "Facebook-Auto-Post-from-JOB_QUEUE" workflow
3. Click "Execute Workflow"
4. Check your Facebook page for the post
5. Check Performance sheet for tracking
```

---

## 📈 Key Features

### Auto-Posting
- Automatically posts from JOB_QUEUE sheet
- Runs every 30 minutes
- Generates short links with Bitly
- Validates URLs before posting
- Tracks post IDs and timestamps

### Retry System
- Detects missed posts automatically
- Reschedules +1 hour
- Up to 10 retry attempts
- Marks as FAILED_FINAL after 10 tries
- Sends alerts on errors

### Real-time Monitoring
- See posts status in dashboard
- Track commission earnings
- Monitor workflow performance
- View error logs

---

## 📤 Data Flow

```
Google Sheets (JOB_QUEUE)
         ⬇️
    N8N Workflow 1
      (Get & Validate)
         ⬇️
  Facebook Post API
         ⬇️
Facebook Page Post
         ⬇️
Google Sheets (Performance)
      (Track Results)
```

---

## 🔨 API Keys You Need

### 1. Facebook Graph API
- Get from: https://developers.facebook.com/
- Need: Page Access Token
- Permission: publish_pages, manage_pages

### 2. Bitly API
- Get from: https://bitly.com/a/sign_up
- Need: API Token
- Format: Bearer {token}

### 3. Google Sheets OAuth
- Get from: Google Cloud Console
- Need: Service Account Key JSON
- Scopes: spreadsheets

---

## 👋 Support & Troubleshooting

### Workflow Not Running?
- Check if workflows are activated (should show green)
- Check N8N execution logs
- Verify API credentials are valid
- Check Google Sheets has correct sheet name

### Posts Not Appearing?
- Check Facebook page permissions
- Verify image URLs are working
- Check rate limits (Facebook allows ~1 post per 10 seconds)
- Check Performance sheet for error messages

### Commission Not Tracking?
- Ensure Performance sheet tab exists
- Check column names match exactly
- Verify webhook from Facebook is configured

---

## 📁 File Structure

```
📁 TUDEALS-Pro-Master-Control-v2.1
├📄 index.html (Dashboard)
├📄 QUICK_START.md (This file)
├📄 SETUP_GUIDE.md (Detailed setup)
├📄 API_CONFIG.md (API documentation)
├📄 TROUBLESHOOTING.md (Common issues)
├📄 Facebook-AutoPost-PUBLISH-SCHEDULED-v3.2__READY__ACTIVE.json
├📄 Facebook-Auto-Post-from-JOB_QUEUE-v1.0.json
├📄 N8N_SETUP.md
├📄 GSHEETS_TEMPLATE.md
├📄 LICENSE
└📄 README.md
```

---

## 🔍 Next Steps

1. ✅ Deploy workflows
2. ✅ Configure Google Sheets
3. ✅ Add API keys
4. ✅ Test with 1 product
5. ✅ Scale to 100+ products
6. ✅ Monitor and optimize

---

## 👉 Ready to Deploy?

**Click "Actions" tab above to see deployment status**

Or visit: https://github.com/Apichat24242424/TUDEALS-Pro-Master-Control-v2.1

---

*Last Updated: 2025-12-29 | Status: 🟢 PRODUCTION READY*