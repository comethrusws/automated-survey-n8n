# Enhanced Survey Chat System - Setup Guide

## Quick Start

### 1. Import the Workflow

1. Open your n8n instance
2. Go to **Workflows** → **Import from File**
3. Select `automated_survey_v2.json`

### 2. Configure Credentials

#### Google Sheets
1. Go to **Settings** → **Credentials** → **Add Credential**
2. Choose **Google Sheets OAuth2 API**
3. Follow the OAuth flow to connect your Google account
4. Apply this credential to all Google Sheets nodes

#### Gemini API
1. Go to **Settings** → **Credentials** → **Add Credential**
2. Choose **Header Auth**
3. Set:
   - **Name**: `x-goog-api-key`
   - **Value**: Your Gemini API key
4. Apply to all HTTP Request nodes

### 3. Create Google Sheet Tabs

Create these tabs in your Google Sheet:

| Tab Name | gid | Headers |
|----------|-----|---------|
| Form Responses | 0 | (Linked to Google Form) |
| Raw Responses | 1 | Timestamp, [All form fields] |
| Cleaned Responses | 2 | Timestamp, ResponseID, Question, OriginalAnswer, TransformedAnswer, Sentiment, Severity, ProcessedAt, Summary, KeyThemes, ActionItems |
| Questions | 3 | Number, Question, Category, Type, UpdatedAt |

### 4. Update Sheet IDs

In the workflow, find all nodes with `YOUR_SHEET_ID` and replace with your actual Google Sheet ID.

> **Tip**: Your Sheet ID is the long string in the URL:  
> `https://docs.google.com/spreadsheets/d/`**`YOUR_SHEET_ID`**`/edit`

### 5. Set Up Chat Interface

1. Open `survey_chat_interface.html` in a browser
2. Click the ⚙️ settings button
3. Enter your webhook URL:
   ```
   https://your-n8n-instance.com/webhook/chat-feedback
   ```
   or 
    ```
    http://localhost:5678/webhook/chat-feedback
    ```
4. Click **Save**

### 6. Activate the Workflow

1. Toggle the workflow to **Active**
2. The chat webhook will now be live

---

## How It Works

```
Google Form → Form Responses Sheet
                    ↓
            n8n Trigger (polls every minute)
                    ↓
            Transform Feedback (Gemini AI)
                    ↓
            Store in Cleaned Responses
                    ↓
            Available for Chat Interface
```

---

## Chat Interface Features

- **Session Memory**: Conversations persist across page refreshes
- **Quick Actions**: One-click access to common queries
- **Real-time Stats**: See sentiment breakdown
- **Dark Mode**: Easy on the eyes

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| CORS errors | Make sure the workflow is active and webhook URL is correct |
| No responses | Check that Form Responses sheet has data and trigger is polling |
| AI not responding | Verify Gemini API key is set correctly |
| Empty stats | Submit at least one form response to populate data |

---

## Files Included

- `automated_survey_v2.json` - n8n workflow
- `survey_chat_interface.html` - Chat UI (open in browser)
- `SETUP_GUIDE.md` - This file
