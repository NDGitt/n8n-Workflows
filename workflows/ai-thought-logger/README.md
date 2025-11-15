# AI Thought Logger

Automatically capture, transcribe, and organize your thoughts from Telegram (voice notes or text) into Google Sheets with AI-powered tagging and summarization.

## 🎯 What This Workflow Does

This workflow automatically:
1. Receives your thoughts via Telegram (text messages or voice notes)
2. Transcribes voice notes using OpenAI Whisper
3. Generates a title, summary, and tags using AI (GPT-4)
4. Logs everything into Google Sheets with timestamps
5. Sends a confirmation message back to your Telegram bot

## 📋 Prerequisites

Before setting up this workflow, you'll need:

- **Telegram Account** + a Telegram bot (created via [BotFather](https://t.me/botfather))
- **n8n Account** (free cloud account or self-hosted)
- **Google Account** with access to Google Sheets
- **OpenAI Account** with API access
  - [How to get OpenAI API Key](https://www.youtube.com/watch?v=OB99E7Y1cMA)

## 🚀 Setup Instructions

### Step 1: Create a Telegram Bot

1. Open Telegram and search for **BotFather**
2. Type `/newbot` and follow the instructions to name your bot
3. BotFather will give you a **Bot Token** - copy this, you'll need it in n8n

**Need help?** Watch this guide: [How to create a Telegram bot](https://www.youtube.com/watch?v=Li2xrdRYP5o)

### Step 2: Prepare Google Sheet

1. Create a new Google Sheet
2. Rename the first tab to **Sheet1**
3. Add these column headers in Row 1:
   - `Timestamp`
   - `Title`
   - `Summary`
   - `Transcript`
   - `Tags`
4. Copy the Sheet ID from the URL (the long string between `/d/` and `/edit`)

**Example:**
```
https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/edit
                                    ↑ This is your Sheet ID ↑
```

### Step 3: Connect Accounts in n8n

1. Go to **Credentials** in the left menu
2. Add the following credentials:
   - **Telegram API** - Paste your Bot Token from BotFather
   - **Google Sheets OAuth2** - Log in with your Google account and allow access
   - **OpenAI API** - Paste your API key from [platform.openai.com](https://platform.openai.com)

### Step 4: Import the Workflow

1. In n8n, click **Workflows** → **Import from File/Clipboard**
2. Upload the `workflow.json` file from this folder
3. Save the workflow

### Step 5: Update Sheet Links

In the workflow, find these nodes and update the Google Sheet ID:
- `Pre - Get row(s) in sheet`
- `Post - Get row(s) in sheet1`
- `Log Thought into Sheet`

Replace the Sheet ID with your own (from Step 2).

### Step 6: Activate and Test

1. Save your workflow
2. Click **Activate**
3. Open Telegram and send your bot a voice note or text message
4. Check:
   - Your Google Sheet → A new row should appear
   - Your Telegram bot → It should reply with confirmation

## 🔧 Workflow Structure

```
Telegram Trigger
    ↓
Check if voice note?
    ├─ Yes → Get file → Transcribe → Merge
    └─ No → Use text message → Merge
    ↓
AI Processing (Generate Title, Summary, Tags)
    ↓
Log to Google Sheets
    ↓
Send Telegram Confirmation
```

## 📊 Google Sheets Format

The workflow creates rows with the following columns:

| Timestamp | Title | Summary | Transcript | Tags |
|-----------|-------|---------|------------|------|
| Auto-generated | AI-generated | AI-generated | Your message/transcription | AI-generated (3-5 tags) |

## 🎨 Customization

- **AI Model**: Change the model in the "Message a model" node (default: `gpt-4o-mini`)
- **Response Messages**: Customize the Telegram confirmation messages in the reply nodes
- **Sheet Columns**: Modify the column mapping in "Log Thought into Sheet" node

## ⚠️ Troubleshooting

- **Bot not responding**: Check that the workflow is activated and the Telegram credentials are correct
- **Sheet not updating**: Verify the Sheet ID is correct and Google Sheets credentials have proper permissions
- **Transcription failing**: Ensure your OpenAI API key is valid and has credits
- **AI not generating tags**: Check the OpenAI model node configuration

## 📝 Notes

- The workflow uses row counting to verify successful sheet updates
- Voice notes are automatically transcribed using OpenAI Whisper
- All timestamps are generated automatically when logging to sheets

## 🔗 Related Resources

- [n8n Telegram Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.telegram/)
- [n8n Google Sheets Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets/)
- [OpenAI API Documentation](https://platform.openai.com/docs)

---

**Enjoy automating your thought capture! 🧠✨**

