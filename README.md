# ColdOutReachAI ✉️
### *Upload a LinkedIn profile screenshot. Get a personalized cold outreach email in 60 seconds.*

An AI-powered cold outreach generator for senior professionals. Paste a job description, upload a LinkedIn profile screenshot, and ColdOutReachAI generates a targeted, personalized cold email - ready to review and send as a Gmail draft.

Works standalone (Path 2) or as a companion to [ResumeTailorAI](https://github.com/anujapol/ResumeTailorAI) (Path 1).

---

## The Problem

Writing a good cold outreach email takes 20-30 minutes when done properly:
- Research the person's background, tenure, shared connections
- Map your experience to the role's top 3 requirements
- Mirror the company's language from the JD
- Write a subject line that gets opened
- Balance confidence with humility

Most people either skip it entirely or send generic messages that get ignored. Neither works.

---

## The Solution

ColdOutReachAI automates the research and drafting using:
- **Claude AI** - reads LinkedIn screenshots via Vision, analyzes the JD, maps candidate experience to role requirements
- **n8n** - orchestrates the form, AI call, and Gmail draft creation
- **Gmail API** - creates a draft ready to review and send

One form submission. 60 seconds. A personalized email that references the person's actual background.

---

## Two Paths

**Path 1 - ResumeTailorAI users**
Your resume is already stored in n8n Data Tables from ResumeTailorAI setup.
Just paste the JD, upload the LinkedIn screenshots, and go.

**Path 2 - Standalone**
Upload your resume (PDF or Word), paste the JD, upload the LinkedIn screenshots.
No other setup required beyond this workflow.

---

## How It Works

```
[Form: Path + JD + LinkedIn screenshots (+ resume for Path 2)]
→ [Prepare Fields]
→ [Path Switch: load resume from Data Table (Path 1) OR use uploaded resume (Path 2)]
→ [Merge]
→ [Claude AI: Vision reads LinkedIn screenshots + generates email]
→ [Extract subject + body]
→ [Display email in form confirmation]
→ [Click button → Create Gmail Draft]
```

---

## What the Email Looks Like

Every generated email follows this structure:

| Section | Content |
|---------|---------|
| Subject | Background + fit + role title |
| Opening | Dear [Name], + mission alignment + personal why |
| Body | 3 bullets mapping your experience to the role's top 3 requirements |
| Close | How your expertise solves their specific challenge + recruiter handoff line |
| Signature | Name, email, phone, LinkedIn |

Total length: 200-250 words. No fluff. No generic claims.

---

## Tech Stack

```
n8n Cloud          — workflow automation and form hosting
Anthropic Claude   — LLM + Vision for profile analysis and email generation
Gmail API          — OAuth2 draft creation
n8n Data Tables    — stores system prompt and resume (Path 1)
```

---

## Repo Structure

```
ColdOutReachAI/
├── workflow.json                        # n8n workflow — import directly
├── outreach_system_prompt_template.txt  # Claude system prompt — fill in your details
└── setup/
    ├── data_table_setup.md              # n8n Data Table configuration
    └── gmail_setup.md                   # Gmail OAuth2 setup (Google Cloud + n8n)
```

---

## Prerequisites

| Service | Free tier? | Used for |
|---------|-----------|----------|
| n8n Cloud | Yes (Starter) | Workflow automation |
| Anthropic API | Pay-as-you-go (~$0.01/email) | AI email generation + LinkedIn Vision |
| Google Account | Yes | Gmail draft creation |

If you are already using ResumeTailorAI, you have everything except the Gmail credential.

---

## Setup Guide

### Step 1 — Import the Workflow

1. In n8n: Workflows → Import → upload `workflow.json`
2. Open the workflow

### Step 2 — Set Up Gmail OAuth2

Follow [`setup/gmail_setup.md`](setup/gmail_setup.md) to:
1. Enable Gmail API in Google Cloud Console
2. Create OAuth2 credentials
3. Add the credential in n8n
4. Update the Create Gmail Draft node

### Step 3 — Add System Prompt to Data Table

1. Open `outreach_system_prompt_template.txt`
2. Fill in your personal details (name, email, phone, LinkedIn, title)
3. Add a row to your n8n `agent_config` Data Table:
   - key: `outreach_system_prompt`
   - value: paste your filled-in system prompt

See [`setup/data_table_setup.md`](setup/data_table_setup.md) for full instructions.

### Step 4 — Configure the Workflow

Update these values in the imported workflow:

| Node | Field | What to set |
|------|-------|-------------|
| **Read Config (Path 1)** | URL | Replace `YOUR_N8N_SUBDOMAIN` and `YOUR_DATA_TABLE_NANOID` |
| **Read Config (Path 1)** | Credential | Your n8n API Key Header Auth credential |
| **Generate Outreach Email** | Credential | Your Anthropic API credential |
| **Create Gmail Draft** | Credential | Your Gmail OAuth2 credential |
| **Show Email** | Response body URL | Replace `YOUR_N8N_SUBDOMAIN` and `YOUR_GMAIL_WEBHOOK_ID` |

To get your Gmail Webhook ID:
1. Click the Gmail Draft Trigger node
2. Copy the webhook path shown - it is auto-generated when you import the workflow

### Step 5 — Activate and Test

1. Toggle the workflow to Active
2. Get the production form URL from the ColdOutReach Form trigger node
3. Submit a test with a real JD and a LinkedIn screenshot
4. Verify the email generates and the Gmail Draft button creates a draft

---

## How to Use

**Taking good LinkedIn screenshots:**
- Screenshot 1: The profile header (name, title, company, location, About section)
- Screenshot 2: The Experience section (optional but improves personalization significantly)
- Standard browser zoom, no cropping needed

**Path 1 users:**
Select "Path 1 — I used ResumeTailorAI" — your resume loads automatically from the data table.

**Path 2 users:**
Select "Path 2 — Standalone" and upload your resume as PDF or Word doc.

---

## Customizing the Email Template

The system prompt controls everything about the email structure, tone, and rules.
To change the format, length, or style:

1. Edit your `outreach_system_prompt` row in the n8n Data Table
2. Changes apply immediately on the next form submission
3. No workflow or code changes needed

---

## Common Issues

| Issue | Cause | Fix |
|-------|-------|-----|
| Gmail draft not created | Gmail OAuth2 not connected | Follow `setup/gmail_setup.md` fully |
| Email missing personalization | Only one screenshot uploaded | Upload both header and experience screenshots |
| Path 1 resume not loading | Data table nanoid wrong in Read Config URL | Fetch `/api/v1/data-tables` to get correct nanoid |
| Claude returns generic email | System prompt placeholder not filled in | Update `outreach_system_prompt` in data table with your actual profile |

---

## Cost

| Service | Cost |
|---------|------|
| n8n Cloud Starter | Free |
| Anthropic API (Vision + generation, ~2000 tokens/email) | ~$0.01/email |
| Gmail API | Free |
| **Total** | **~$0.10/month** for 10 emails |

---

## Used with ResumeTailorAI?

This workflow is designed as a companion to [ResumeTailorAI](https://github.com/anujapol/ResumeTailorAI).

Typical workflow:
1. ResumeTailorAI generates your tailored resume for a role
2. ColdOutReachAI generates your cold outreach email to the hiring manager
3. Review both, approve, apply and send

---

## Roadmap

**v2 - Follow-up email generator**
Generate follow-up emails at Day 3, Day 7, Day 12 based on your tracker data - automatically triggered from your job application tracker.

---

## License

MIT — use freely, adapt for your own outreach, and build on top of it.

---

*Built to solve a real problem. Tested on real outreach.*
