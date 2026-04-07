---
name: google-workspace
description: Create, read, edit, and share Google Docs, Sheets, Drive files, and Calendar events via `gog` CLI. Use when tasks involve creating documents, spreadsheets, managing files in Google Drive, scheduling, or working with Google Workspace content. Covers the coreconx.group domain.
---

# Google Workspace

All Google Workspace operations use the `gog` CLI (v0.12.0), authenticated via OAuth2 for `chuck@coreconx.group`.

**No custom scripts needed.** `gog` handles Docs, Sheets, Drive, and Calendar natively.

## Golden Rule

Always prefix commands with: `gog -a chuck@coreconx.group`

Run `gog <service> <command> --help` before guessing flags.

## Default Sharing

**Always share every created document with `dylan@coreconx.group`** (role: writer).

After creating any doc/sheet/file:
```bash
gog -a chuck@coreconx.group drive share <fileId> --email dylan@coreconx.group --role writer
```

## Google Docs

```bash
# Create a new doc
gog -a chuck@coreconx.group docs create "Title Here"

# Create with content from file
gog -a chuck@coreconx.group docs create "Title" --content "$(cat drafts/doc.md)"

# Read doc as plain text
gog -a chuck@coreconx.group docs cat <docId>

# Write content to doc
gog -a chuck@coreconx.group docs write <docId> --content "New content"

# Insert text at position
gog -a chuck@coreconx.group docs insert <docId> "Text to insert"

# Find and replace
gog -a chuck@coreconx.group docs edit <docId> "old text" "new text"

# Export as markdown/pdf/docx/txt
gog -a chuck@coreconx.group docs export <docId> --format md

# View structure
gog -a chuck@coreconx.group docs structure <docId>
```

## Google Sheets

```bash
# Create spreadsheet
gog -a chuck@coreconx.group sheets create "Title"

# Read range
gog -a chuck@coreconx.group sheets get <spreadsheetId> "Sheet1!A1:D10"

# Update cells
gog -a chuck@coreconx.group sheets update <spreadsheetId> "Sheet1!A1" "value1" "value2"

# Append row
gog -a chuck@coreconx.group sheets append <spreadsheetId> "Sheet1!A:D" "val1" "val2" "val3"

# Get spreadsheet info
gog -a chuck@coreconx.group sheets metadata <spreadsheetId>

# Export as csv/xlsx/pdf
gog -a chuck@coreconx.group sheets export <spreadsheetId> --format csv

# Add a new tab
gog -a chuck@coreconx.group sheets add-tab <spreadsheetId> "New Tab Name"
```

## Google Drive

```bash
# List files in root
gog -a chuck@coreconx.group drive ls

# Search files
gog -a chuck@coreconx.group drive search "CoreConX"

# Get file metadata
gog -a chuck@coreconx.group drive get <fileId>

# Download file
gog -a chuck@coreconx.group drive download <fileId>

# Upload file
gog -a chuck@coreconx.group drive upload ./report.pdf --parent <folderId>

# Create folder
gog -a chuck@coreconx.group drive mkdir "Onboarding" --parent <folderId>

# Share file
gog -a chuck@coreconx.group drive share <fileId> --email dylan@coreconx.group --role writer

# Delete (trash) — CONFIRM FIRST
gog -a chuck@coreconx.group drive delete <fileId>
```

## Google Calendar

```bash
# List upcoming events
gog -a chuck@coreconx.group calendar events

# Events in date range
gog -a chuck@coreconx.group calendar events <calendarId> --from 2026-04-07 --to 2026-04-14

# Search events
gog -a chuck@coreconx.group calendar search "standup"

# Create event — CONFIRM WITH DYLAN FIRST
gog -a chuck@coreconx.group calendar create <calendarId> --summary "Meeting" --start "2026-04-08T10:00:00" --end "2026-04-08T11:00:00"

# Check availability
gog -a chuck@coreconx.group calendar freebusy

# Find conflicts
gog -a chuck@coreconx.group calendar conflicts
```

## Useful Flags

| Flag | Purpose |
|------|---------|
| `-j` | JSON output (best for scripting/parsing) |
| `-p` | Plain TSV output |
| `-n` | Dry-run (don't actually change anything) |
| `-y` | Skip confirmations |
| `--no-input` | Never prompt, fail instead |
| `--max <n>` | Limit results |

## Safety Rules

- **Auto-share with Dylan**: Every created file → share with `dylan@coreconx.group` as writer
- **No public sharing**: Never set "anyone with the link" without explicit approval
- **No delete without confirm**: Always ask before deleting Drive files
- **Drafts locally first**: For large documents, draft in workspace before pushing to Docs
- **Calendar creates = Level 1**: Always confirm with Dylan before creating events
- **Never log OAuth tokens** in chat
