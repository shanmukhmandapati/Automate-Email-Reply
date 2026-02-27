# Automate-Email-Reply

An AI-powered Gmail auto-reply workflow built with n8n and OpenAI. It reads incoming emails, detects the tone, and sends a warm, context-aware reply — automatically.

## What It Does

- Monitors your Gmail inbox every minute
- Filters only personal emails (ignores promotions, newsletters, spam)
- Detects the tone of the email using AI
- Generates a human-like reply that matches the tone
- Sends the reply directly back to the email thread

## Workflow

```
Gmail Trigger (every minute)
     ↓
If — is it a personal email?
     ↓ yes
Detect Sentiment (OpenAI)
     ↓
FORMAL / CASUAL / URGENT / ANGRY / SAD / FRIENDLY
     ↓
AI Agent — writes tone-matched reply
     ↓
Gmail — sends reply to original thread
```

## Tone Matching Rules

| Detected Tone | Reply Style |
|---------------|-------------|
| FORMAL | Professional, proper greeting |
| CASUAL | Conversational, light |
| URGENT | Concise, acknowledges urgency |
| ANGRY | Calm, empathetic |
| SAD | Warm, compassionate |
| FRIENDLY | Personal, engaging |

## Guardrails

- Replies kept under 100 words unless email is complex
- Signs off as **Shanmukh**
- Never responds to emails about money, investments, stocks, or gambling
- Never shares personal data or passwords
- Only processes `CATEGORY_PERSONAL` emails

## Tech Stack

| Tool | Purpose |
|------|---------|
| n8n Cloud | Workflow automation |
| Gmail (OAuth2) | Trigger + send reply |
| OpenAI GPT-4.1 mini | Sentiment detection + reply generation |

## n8n Nodes

**1. Gmail Trigger**
- Polls every minute
- Watches: INBOX
- Filter: personal emails only

**2. If**
- Condition: `labelIds` contains `CATEGORY_PERSONAL`
- Skips promotional, social, and update emails

**3. Detect Sentiment**
- LLM Chain node
- Prompt: analyze email tone
- Returns one word: FORMAL, CASUAL, URGENT, ANGRY, SAD, or FRIENDLY

**4. AI Agent**
- Reads email content + detected tone
- Generates reply following tone rules and guardrails
- Model: GPT-4.1 mini

**5. Reply to a message**
- Gmail node
- Sends AI-generated reply to original email thread


## Setup

### 1. Import the workflow into n8n

1. Open [n8n Cloud](https://app.n8n.cloud)
2. Click **"+"** → **"New Workflow"**
3. Click `...` → **"Import from file"**
4. Select `n8n/automate-email-reply.json`

### 2. Connect your credentials

- **Gmail OAuth2** — connect your Google account
- **OpenAI API** — add your OpenAI API key

### 3. Update the sender filter (optional)

In the Gmail Trigger node, update the sender filter to your own email or remove it to monitor all incoming emails.

### 4. Activate the workflow

Click **"Publish"** to activate. The workflow will now run every minute automatically.


## Important Notes

- Make sure your Gmail account has OAuth2 access enabled in n8n
- Test in n8n test mode first before activating
- Monitor the first few executions to make sure replies look right
- The workflow only replies to `CATEGORY_PERSONAL` — Gmail must correctly categorize incoming emails for this to work


## Built As Part Of

This workflow was built during the **AI Product Course** on Maven, instructed by Mahesh Yadav — as part of learning to automate real workflows using AI and no-code tools.


## License

MIT
