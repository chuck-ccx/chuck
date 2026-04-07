---
name: gmail
description: Read, send, draft, and manage emails via `gog` CLI. Use when tasks involve email — reading inbox, sending messages, creating drafts, managing labels, searching threads, or checking for unread mail. Covers chuck@coreconx.group. Triggers on email-related requests, inbox checks, outreach drafts, customer replies, and heartbeat email polling.
---

# Gmail Skill

Gmail access via `gog` CLI (v0.12.0), authenticated via OAuth2 for `chuck@coreconx.group`.

## Quick Reference

All commands prefixed with: `gog -a chuck@coreconx.group gmail`

```bash
# Search/list messages (Gmail query syntax)
gog -a chuck@coreconx.group gmail search "is:unread" --max 10

# Read a specific message
gog -a chuck@coreconx.group gmail get <messageId>

# Send email — LEVEL 1: Draft preview + Dylan approval first
gog -a chuck@coreconx.group gmail send --to "user@example.com" --subject "Subject" --body "Body"

# Create draft (safe — does NOT send)
gog -a chuck@coreconx.group gmail drafts create --to "user@example.com" --subject "Subject" --body "Body"

# List labels
gog -a chuck@coreconx.group gmail labels list

# Archive message
gog -a chuck@coreconx.group gmail archive <messageId>

# Mark as read
gog -a chuck@coreconx.group gmail mark-read <messageId>

# Download attachment
gog -a chuck@coreconx.group gmail attachment <messageId> <attachmentId>
```

## Query Operators

Use in search queries:

- `is:unread` / `is:starred` / `is:important`
- `from:` / `to:` / `subject:`
- `after:YYYY/MM/DD` / `before:YYYY/MM/DD`
- `has:attachment`
- `label:` / `category:`
- `larger:5M` / `smaller:1M`
- Combine with spaces (AND) or `OR`

## Safety Rules

- **Drafts first**: Always create a draft and get approval before sending external email (Level 1)
- **No bulk sends**: Never loop-send to multiple recipients without explicit approval
- **No auto-forward**: Never create forwarding rules or filters autonomously
- **No trash without confirm**: Always ask before trashing messages
- **Secrets**: Never log or display OAuth tokens or full email bodies in chat unless asked

## Useful Flags

| Flag | Purpose |
|------|---------|
| `-j` | JSON output |
| `-p` | Plain TSV output |
| `--max <n>` | Limit results |
| `-n` | Dry-run |
