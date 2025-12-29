# 📄 TUDEALS Pro - Complete Setup Guide

## Prerequisites

- [ ] N8N instance (self-hosted or cloud)
- [ ] Google Account (for Google Sheets & OAuth)
- [ ] Facebook Business Account
- [ ] Bitly Account
- [ ] Text editor or IDE

---

## 🔗 1. Google Sheets Setup

### Create New Spreadsheet

1. Go to https://sheets.google.com
2. Create new spreadsheet
3. Name: `TUDEALS-JOB-QUEUE-PROD`
4. Create two sheets:
   - `JOB_QUEUE` (for products to post)
   - `Performance` (for tracking results)

### JOB_QUEUE Sheet Structure

| Column | Type | Example | Required |
|--------|------|---------|----------|
| ID | Number | 1 | Yes |
| product_name | Text | iPhone 15 | Yes |
| price | Number | 25000 | Yes |
| image_url | URL | https://... | Yes |
| shop_link | URL | https://shopee... | Yes |
| status | Text | APPROVED | Yes |
| scheduled_time | DateTime | 2025-12-29 18:00 | Yes |
| retry_count | Number | 0 | Auto |
| last_error | Text | (empty) | Auto |
| post_id | Text | (empty) | Auto |

### Performance Sheet Structure

| Column | Type | Example | Required |
|--------|------|---------|----------|
| ID | Number | 1 | Yes |
| job_id | Number | 1 | Yes |
| post_id | Text | 123456789 | Auto |
| platform | Text | facebook | Auto |
| post_time | DateTime | 2025-12-29 18:05 | Auto |
| revenue | Number | 2500 | Auto |
| commission | Number | 250 | Auto |
| status | Text | POSTED | Auto |
| shortlink | Text | https://bit.ly/... | Auto |

---

## 📤 2. Facebook Setup

### Create Facebook Business Account

1. Go to https://business.facebook.com
2. Create/Login to business account
3. Go to Settings > Users
4. Create system user (for API access)

### Get Page Access Token

1. Go to https://developers.facebook.com/tools/explorer/
2. Select your page from dropdown
3. Click on "Generate Access Token"
4. Choose "User Token" or "Page Token"
5. Copy the token (starts with `EAAB...`)
6. **Important**: Go to Settings > Tokens and make it permanent

### Required Permissions

Make sure these are enabled:
- [ ] pages_read_engagement
- [ ] pages_manage_posts
- [ ] pages_read_user_content

---

## 🔗 3. Bitly API Setup

### Get Bitly API Key

1. Go to https://bitly.com/a/
2. Login or sign up
3. Go to Account Settings
4. Click "API" or "Developer" section
5. Generate new access token
6. Copy the token (format: `api_key_xxxxx...`)

### Test Bitly API

```bash
curl -X POST https://api-ssl.bitly.com/v4/shorten \
  -H "Authorization: Bearer YOUR_BITLY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"long_url": "https://example.com", "domain": "bit.ly"}'
```

---

## 🔚 4. N8N Workflow Setup

### Import Workflows

1. Open N8N Dashboard
2. Click "Workflows" menu
3. Click "Import from File"
4. Select: `Facebook-AutoPost-PUBLISH-SCHEDULED-v3.2__READY__ACTIVE.json`
5. Click "Import"
6. Repeat for: `Facebook-Auto-Post-from-JOB_QUEUE-v1.0.json`

### Configure Credentials

#### Google Sheets Credential

1. In N8N, go to Credentials
2. Click "New" > "Google Sheets"
3. Name: `Google Sheets - TUDEALS`
4. Click "Connect my account"
5. Login to Google and authorize
6. Save

#### Facebook Credential

1. In N8N, go to Credentials
2. Click "New" > "Facebook Graph API"
3. Name: `Facebook - TUDEALS Page`
4. Paste your Page Access Token
5. Save

#### Bitly Credential

1. In N8N, go to Credentials
2. Click "New" > "HTTP Header Auth"
3. Name: `Bitly API Token`
4. Headers:
   - Key: `Authorization`
   - Value: `Bearer YOUR_BITLY_TOKEN`
5. Save

### Configure Workflow 1: Requeue Trigger

1. Open `Facebook-AutoPost-PUBLISH-SCHEDULED-v3.2` workflow
2. Edit nodes:
   - **Google Sheets node**: Select credential "Google Sheets - TUDEALS"
   - Set Spreadsheet ID to your JOB_QUEUE sheet ID
   - Verify column names match
3. **Trigger**: Set to "Every 5 minutes"
4. **Test Execute** to verify it works
5. Click "Activate Workflow"

### Configure Workflow 2: Auto Post Pipeline

1. Open `Facebook-Auto-Post-from-JOB_QUEUE` workflow
2. Edit nodes:
   - **Google Sheets node**: Select credential "Google Sheets - TUDEALS"
   - **Bitly node**: Configure with your API endpoint
   - **Facebook node**: Configure with Page ID and token
3. **Trigger**: Set to "Every 30 minutes"
4. **Test Execute** to verify it works
5. Click "Activate Workflow"

---

## 📄 5. Test & Verification

### Test 1: Manual Execution

```bash
1. Go to JOB_QUEUE sheet
2. Add test product:
   - product_name: "Test Product"
   - price: 1000
   - image_url: "https://via.placeholder.com/500"
   - shop_link: "https://shopee.co.th/"
   - status: "APPROVED"
   - scheduled_time: current time + 1 min
3. Go to N8N > Facebook-Auto-Post workflow
4. Click "Execute Workflow"
5. Check your Facebook page for the post
```

### Test 2: Check Performance Sheet

```bash
1. Go to Performance sheet
2. Should see new row with:
   - post_id: (Facebook post ID)
   - status: "POSTED"
   - shortlink: (Bitly short URL)
```

### Test 3: Verify Retry Logic

```bash
1. Manually mark a post as "MISSED" in JOB_QUEUE
2. Wait 5 minutes for Requeue Trigger to run
3. Check if status changed back to "APPROVED"
4. Check if scheduled_time was +1 hour
```

---

## 📁 6. Production Checklist

- [ ] All credentials configured
- [ ] Both workflows activated
- [ ] Test posts working
- [ ] Performance sheet tracking correctly
- [ ] Error handling verified
- [ ] Retry logic tested
- [ ] Rate limits understood
- [ ] Backup of workflow JSONs saved
- [ ] Monitoring dashboard set up
- [ ] Team trained on usage

---

## 🔝 Troubleshooting

### Workflow Not Activating

```
Check:
1. All credentials are valid (click test)
2. Spreadsheet ID is correct
3. Sheet names are exact match
4. API tokens haven't expired
```

### Posts Not Appearing

```
Check:
1. Page Access Token is still valid
2. Facebook page permissions are enabled
3. Image URLs are accessible
4. Rate limiting (max 1 post per 10 sec)
5. N8N execution logs for errors
```

### Performance Sheet Not Updating

```
Check:
1. Performance sheet exists and is named exactly
2. Column names match workflow configuration
3. Workflow has write permissions to sheet
4. Check N8N error logs
```

---

## 💺 Support

**GitHub Issues**: https://github.com/Apichat24242424/TUDEALS-Pro-Master-Control-v2.1/issues

**Documentation**: Check other .md files in this repo

---

*Last Updated: 2025-12-29 | Status: 🟢 PRODUCTION READY*