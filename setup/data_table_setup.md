# ColdOutReachAI — Data Table Setup

ColdOutReachAI uses the same n8n Data Tables pattern as ResumeTailorAI.
Path 1 users share the existing agent_config table. Path 2 users only need
the outreach_system_prompt row — no resume in the table.

---

## Path 1 Users (ResumeTailorAI already set up)

Add one new row to your existing agent_config Data Table:

| key | value |
|-----|-------|
| `outreach_system_prompt` | Paste contents of `outreach_system_prompt_template.txt` with your details filled in |

Your resume_primary row is already there — ColdOutReachAI reads it automatically.

---

## Path 2 Users (Standalone)

1. In n8n: Data → Tables → Add Table → name it `agent_config`
2. Add columns: `key` (String) and `value` (Text)
3. Add one row:

| key | value |
|-----|-------|
| `outreach_system_prompt` | Paste contents of `outreach_system_prompt_template.txt` with your details filled in |

Note: Path 2 users upload their resume directly in the form — no resume_primary row needed.

---

## Get Your Data Table Nanoid

Same process as ResumeTailorAI:

```
GET https://YOUR_SUBDOMAIN.app.n8n.cloud/api/v1/data-tables
Header: X-N8N-API-KEY: your-api-key
```

Find agent_config in the response, copy its id value.
Paste into the Read Config (Path 1) node URL.
