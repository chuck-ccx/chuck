---
name: obsidian
description: Read, search, create, move, and manage notes in the Obsidian vault. Use when tasks involve knowledge base lookups, research notes, project documentation, or organizing information in Obsidian. Triggers on requests to find notes, save research, check documentation, or manage the vault.
---

# Obsidian Skill

Obsidian vault = a normal folder of Markdown files on disk. Notes are plain `.md` files; edit directly or use `obsidian-cli` when link-aware operations matter.

## Vault Discovery

**Do not hardcode vault paths.** Resolve on first use:

1. Read `~/Library/Application Support/obsidian/obsidian.json`
2. Find the vault entry with `"open": true` — that's the active vault
3. The vault path is the key in the `vaults` object

Cache the resolved path for the session. If multiple vaults exist, ask Dylan which one.

## Tools

### Direct File Access (preferred for reads/edits)

- **Read:** Open the `.md` file directly. Obsidian picks up external changes automatically.
- **Edit:** Modify the file in place. Obsidian live-reloads.
- **Search:** Use `grep`/`glob` on the vault directory for fast lookups.

### obsidian-cli (when installed)

> **Note:** `obsidian-cli` may not be installed yet. Check with `which obsidian-cli` first. If unavailable, fall back to direct file operations.

| Command | Purpose |
|---------|---------|
| `obsidian-cli set-default "<vault>"` | Set default vault (once) |
| `obsidian-cli print-default --path-only` | Get vault path |
| `obsidian-cli search "query"` | Search note names |
| `obsidian-cli search-content "query"` | Full-text search with snippets |
| `obsidian-cli create "Folder/Note" --content "..."` | Create note (updates Obsidian URI) |
| `obsidian-cli move "old/path" "new/path"` | Move + update all `[[wikilinks]]` |
| `obsidian-cli delete "path/note"` | Delete note |

**When to use CLI vs direct edit:**
- Use CLI for `move` (wikilink refactoring) and `create` (if you want Obsidian to open the note)
- Use direct file read/write for everything else — it's faster and doesn't depend on the CLI

## Autonomy Rules

| Operation | Level | Rule |
|-----------|-------|------|
| Read / search notes | 0 | Free — do anytime |
| Create notes in designated folders | 0 | Free — research/, drafts/, daily/ |
| Create notes elsewhere | 1 | Ask first |
| Edit existing notes | 1 | Ask first (unless it's a file Chuck created) |
| Move / rename notes | 1 | Ask first — may break links |
| Delete notes | 2 | Always ask + confirm |

## Vault Structure

Typical layout (actual structure may vary — discover from the vault, don't assume):

```
vault/
  .obsidian/          # Config — do not touch
  *.canvas            # Canvas files (JSON) — read-only
  attachments/        # Images, PDFs — varies by settings
  [user folders]/     # Notes organized however Dylan set them up
```

## Safety

- **Never write to `.obsidian/`** — that's Obsidian's config directory
- **Never delete without asking** — use trash/archive over delete
- **Don't create notes in hidden dot-folders** — Obsidian may ignore them
- **Respect existing structure** — don't reorganize without permission
- **No hardcoded paths** — always resolve from config

## Integration with Workspace

When research or project context belongs in both the workspace and the vault:
- Workspace `memory/` is Chuck's working memory (raw, fast, session-scoped)
- Obsidian vault is the knowledge base (curated, persistent, human-readable)
- Don't auto-sync between them — ask Dylan if/when to push notes to the vault
