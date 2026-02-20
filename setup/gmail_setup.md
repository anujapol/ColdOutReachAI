# Gmail OAuth2 Setup Guide

ColdOutReachAI uses Gmail to create draft emails. This requires a Gmail
OAuth2 credential in n8n connected to your Google account.

---

## Step 1: Enable Gmail API in Google Cloud

1. Go to console.cloud.google.com
2. Create a new project (or use an existing one)
3. Go to APIs and Services → Library
4. Search for "Gmail API" → click Enable

---

## Step 2: Create OAuth2 Credentials

1. Go to APIs and Services → Credentials
2. Click Create Credentials → OAuth client ID
3. Application type: Web application
4. Name: ColdOutReachAI
5. Under Authorized redirect URIs, add:
   ```
   https://YOUR_N8N_SUBDOMAIN.app.n8n.cloud/rest/oauth2-credential/callback
   ```
6. Click Create
7. Copy the Client ID and Client Secret

---

## Step 3: Configure OAuth Consent Screen (if not already done)

1. Go to APIs and Services → OAuth consent screen
2. User type: External
3. App name: ColdOutReachAI
4. Add your email as a test user
5. Scopes: add `https://www.googleapis.com/auth/gmail.compose`
6. Save

---

## Step 4: Add Gmail Credential in n8n

1. In n8n: Credentials → New → Gmail OAuth2 API
2. Enter your Client ID and Client Secret from Step 2
3. Click Connect My Account — Google OAuth flow opens
4. Sign in and grant access
5. Name the credential (e.g. Gmail OAuth2) and save
6. Use this credential name in the Create Gmail Draft node

---

## Step 5: Update the Workflow

In the Create Gmail Draft node, update the credential field to match
the name you used in Step 4.

---

## Note on Gmail Draft Behaviour

The workflow creates a draft — it does NOT send the email automatically.
The draft appears in your Gmail Drafts folder with:
- Subject pre-filled
- Body pre-filled with the generated email
- To field empty — add the recipient manually before sending

This is intentional. Always review and personalise before sending.
