---
name: gmail
description: Read, send, draft, and manage emails via Gmail API with direct OAuth2 authentication. Use when tasks involve email — reading inbox, sending messages, creating drafts, managing labels, searching threads, or checking for unread mail. Covers chuck@coreconx.com. Triggers on email-related requests, inbox checks, outreach drafts, customer replies, and heartbeat email polling.
---

# Gmail Skill

Direct Gmail API access using Google OAuth2 service account credentials. No third-party proxy.

## Authentication

Credentials are stored as a service account JSON key file. The path is set via environment variable:

```
GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account.json
```

For domain-wide delegation (required for `chuck@coreconx.com`), the service account must be granted access in Google Workspace admin with these scopes:
- `https://www.googleapis.com/auth/gmail.readonly`
- `https://www.googleapis.com/auth/gmail.send`
- `https://www.googleapis.com/auth/gmail.modify`
- `https://www.googleapis.com/auth/gmail.labels`

The impersonated user is `chuck@coreconx.com`.

## Quick Reference

All endpoints use base URL: `https://gmail.googleapis.com/gmail/v1/users/me`

| Action | Method | Path |
|--------|--------|------|
| List messages | GET | `/messages?maxResults=10` |
| Search messages | GET | `/messages?q=is:unread&maxResults=10` |
| Get message | GET | `/messages/{id}` |
| Send message | POST | `/messages/send` |
| Create draft | POST | `/drafts` |
| Send draft | POST | `/drafts/send` |
| List labels | GET | `/labels` |
| List threads | GET | `/threads?maxResults=10` |
| Get thread | GET | `/threads/{id}` |
| Modify labels | POST | `/messages/{id}/modify` |
| Trash message | POST | `/messages/{id}/trash` |
| Get profile | GET | `/profile` |

## Usage

Use `scripts/gmail.py` for all Gmail operations. It handles OAuth2 token refresh automatically.

```bash
# List unread
python3 scripts/gmail.py list --query "is:unread" --max 10

# Read a message (returns headers + body text)
python3 scripts/gmail.py get <message_id>

# Send email
python3 scripts/gmail.py send --to "user@example.com" --subject "Subject" --body "Body text"

# Create draft (does NOT send)
python3 scripts/gmail.py draft --to "user@example.com" --subject "Subject" --body "Body text"

# Send existing draft
python3 scripts/gmail.py send-draft <draft_id>

# Search with Gmail query operators
python3 scripts/gmail.py list --query "from:someone@example.com after:2026/01/01 has:attachment"
```

## Safety Rules

- **Drafts first**: Always create a draft and get approval before sending external email (Level 1).
- **No bulk sends**: Never loop-send to multiple recipients without explicit approval.
- **No auto-forward**: Never create forwarding rules or filters autonomously.
- **Secrets**: Never log or display OAuth tokens, credentials, or full email bodies in chat unless asked.
- **Rate limit**: Gmail API allows 250 quota units/sec. The script handles 429 retries with exponential backoff.

## Query Operators

Use in the `--query` parameter:

- `is:unread` / `is:starred` / `is:important`
- `from:` / `to:` / `subject:`
- `after:YYYY/MM/DD` / `before:YYYY/MM/DD`
- `has:attachment`
- `label:` / `category:`
- `larger:5M` / `smaller:1M`
- Combine with spaces (AND) or `OR` / `{ }` for grouping

## Setup Checklist

If Gmail is not yet configured, follow these steps:

1. Create a Google Cloud project (or use existing CoreConX project)
2. Enable Gmail API in the project
3. Create a service account with domain-wide delegation
4. Download the JSON key file
5. In Google Workspace Admin > Security > API Controls > Domain-wide Delegation, add the service account client ID with the scopes listed above
6. Set `GOOGLE_APPLICATION_CREDENTIALS` env var to the key file path
7. Test: `python3 scripts/gmail.py list --max 1`

For detailed API reference beyond what the script covers, see `references/api-reference.md`.
