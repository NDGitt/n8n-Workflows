# AI Email Manager

An intelligent Gmail automation workflow that uses AI to automatically categorize emails with custom labels and draft replies for emails requiring responses.

## 🎯 What This Workflow Does

This workflow automatically:

1. **Monitors your Gmail inbox** for new emails (checks every minute)
2. **Categorizes emails** using AI-powered classification into custom categories
3. **Applies Gmail labels** automatically based on email content
4. **Drafts replies** for emails that require your response (TO_RESPOND category)
5. **Evaluates performance** through a built-in testing system

**Features:**
- AI-powered email categorization with customizable rules
- Automatic Gmail label application
- Smart draft generation for emails requiring replies
- Customizable label categories and rules
- Built-in evaluation system for testing accuracy
- Filters out emails already labeled by existing Gmail rules

## 📋 Prerequisites

Before setting up this workflow, you'll need:

- **Gmail Account** with access to Gmail API
- **n8n Account** (free cloud account or self-hosted)
- **OpenAI Account** with API access
  - [How to get OpenAI API Key](https://platform.openai.com/api-keys)
- **Google Sheets Account** (for evaluation/testing - optional)
- **Gmail Labels** created in your Gmail account (see Step 1 below)

## 🚀 Setup Instructions

### Step 1: Create Your Gmail Labels

**⚠️ Important:** This workflow does NOT create labels automatically. You must create them manually in Gmail first.

1. Open Gmail → left sidebar → **Create new label**
2. Add all the labels you want the AI to use (e.g., "Jobs", "To Respond", "Meeting Update", "Marketing", etc.)
3. Note the label names you create - you'll reference them in Step 2

**Example labels you might create:**
- Job Application Updates
- To Respond
- Meeting Updates
- Event Invites
- Newsletters
- Marketing
- Cold Email

### Step 2: Connect Accounts in n8n

1. Go to **Credentials** in the left menu
2. Add the following credentials:
   - **Gmail OAuth2** - Log in with your Gmail account and allow access
   - **OpenAI API** - Paste your API key from [platform.openai.com](https://platform.openai.com)
   - **Google Sheets OAuth2** (optional, for evaluations) - Log in with your Google account

### Step 3: Import the Workflow

1. In n8n, click **Workflows** → **Import from File/Clipboard**
2. Upload the `workflow.json` file from this folder
3. Save the workflow

### Step 4: Configure Your Custom Label Rules (One-Time Setup)

The workflow includes a **setup section** (disabled by default) that helps you create custom label rules:

1. **Enable the setup nodes:**
   - `When chat message received` (chat trigger)
   - `Get many labels`
   - `Code in JavaScript`
   - `Message a model`
   - `Step 2 Output`

2. **Use the chat interface** (left panel in n8n) to describe your label rules:
   - Tell the AI what each label means
   - When each label should apply
   - When it should NOT apply
   - Any special rules or exceptions

   **Example chat message:**
   ```
   I want a marketing label, a jobs label, and a meetings label.
   Marketing is for promos and sales emails.
   Jobs is for application updates and recruiter responses.
   Meetings is for invites or scheduling changes.
   ```

3. **Run the setup workflow** - The AI will generate:
   - `prompt_block`: Your custom categories & labels section
   - `label_mapping`: Gmail Label IDs mapped to categories

4. **Copy the `prompt_block`** from the execution output

### Step 5: Update the Main Email Agent

1. Open the **Main Email Agent** node
2. Find the section starting with `### CATEGORIES & LABELS`
3. Replace that entire section (up to `### TOOLS YOU CAN USE`) with your `prompt_block` from Step 4
4. Update the **GMAIL LABEL IDS** section with your actual label IDs from the `label_mapping`

### Step 6: Configure Label ID Exclusions (Optional)

If you have existing Gmail filters that already label certain emails, you can exclude them:

1. Open the **"Labelled by an existing GMAIL logic?"** node
2. Update the label ID checks to match your existing Gmail filter labels
3. This prevents the AI from re-processing emails already handled by your filters

### Step 7: Activate and Test

1. Save your workflow
2. Click **Activate**
3. Send yourself a test email
4. Check:
   - Your Gmail → The email should have the appropriate label
   - If it's a "To Respond" email → A draft reply should be created

## 🔧 Workflow Structure

The workflow consists of several main components:

### Main Workflow (Active)

```
Gmail Trigger (checks every minute)
    ↓
Labelled by existing Gmail logic? (filter)
    ↓ (if not already labeled)
Main Email Agent (categorizes & labels)
    ↓
Small modifications to JSON
    ↓
Is a response expected? (checks for TO_RESPOND label)
    ├─ Yes → Email Draft Agent → Creates draft reply
    └─ No → End
```

### Setup Workflow (Disabled - One-time use)

```
When chat message received
    ↓
Get many labels (from Gmail)
    ↓
Code in JavaScript (merges user text + labels)
    ↓
Message a model (generates custom prompt)
    ↓
Step 2 Output (extracts prompt_block & label_mapping)
```

### Evaluation Workflow (Optional - Testing)

```
When fetching a dataset row (from Google Sheets)
    ↓
Edit Fields
    ↓
Merge (combines with Main Email Agent output)
    ↓
Eval Output
    ↓
Evaluation1 (writes results back to sheet)
```

## 📊 Default Categories

The workflow comes with these default categories (customizable via Step 4):

1. **JOB_APPLICATION_UPDATE** - Application status updates, interview invitations
2. **NEW_JOB_OPPORTUNITIES** - Job postings and recruiter cold outreach
3. **TO_RESPOND** - Emails requiring your direct response
4. **MEETING_UPDATE** - Meeting invitations, scheduling changes
5. **EVENT_INVITES** - Conference, webinar, social event invitations
6. **EVENT_UPDATES** - Updates about events you're registered for
7. **NEWSLETTERS** - Scheduled newsletters and content digests
8. **COLD_EMAIL** - Unsolicited sales outreach
9. **MARKETING** - Promotional emails from brands you use

## 🎨 Customization

### Changing AI Models

- **Main Email Agent**: Uses `gpt-4o-mini` by default (configured in `OpenAI Chat Model1` node)
- **Email Draft Agent**: Uses `gpt-4o-mini` by default (configured in `OpenAI Chat Model` node)
- **Setup Model**: Uses `gpt-4o-mini` by default (configured in `Message a model` node)

You can change these in the respective OpenAI model nodes.

### Adjusting Email Check Frequency

Edit the **Gmail Trigger** node:
- Change `pollTimes` from `everyMinute` to your preferred interval

### Customizing Draft Reply Style

Edit the **Email Draft Agent** node prompt to change:
- Reply tone and style
- Length preferences
- Greeting/sign-off format

## 📝 Evaluation System (Optional)

The workflow includes an evaluation system to test accuracy:

1. **Create a Google Sheet** with columns:
   - `email_id` - Gmail message ID
   - `email_json` - Full email JSON as string
   - `expected_label_id` - Label you expect
   - `expected_has_draft` - TRUE/FALSE if draft expected

2. **Configure the evaluation nodes:**
   - `When fetching a dataset row` - Point to your Google Sheet
   - `Evaluation1` - Point to the same sheet

3. **Run the evaluation workflow** - It will:
   - Test each email in your sheet
   - Compare predicted vs expected labels
   - Write results back to the sheet

## ⚠️ Troubleshooting

- **Emails not being labeled**: 
  - Check that the workflow is activated
  - Verify Gmail credentials are correct
  - Ensure labels exist in Gmail with matching IDs

- **Drafts not being created**:
  - Verify the email was labeled as "TO_RESPOND"
  - Check the "Is a response expected?" node condition
  - Ensure OpenAI API key is valid

- **Setup workflow not working**:
  - Make sure all setup nodes are enabled
  - Check that you've created labels in Gmail first
  - Verify the chat trigger is accessible

- **Label IDs not matching**:
  - Use the "Get many labels" node to fetch actual label IDs
  - Update the GMAIL LABEL IDS section in Main Email Agent

## 🔗 Related Resources

- [n8n Gmail Node Documentation](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.gmail/)
- [n8n AI Agent Documentation](https://docs.n8n.io/integrations/builtin/ai/n8n-nodes-langchain.agent/)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Gmail Labels API](https://developers.google.com/gmail/api/guides/labels)

---

**Enjoy automated email management! 📧✨**
