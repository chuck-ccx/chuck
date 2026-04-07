# CoreConX System Architecture

## Runtime & Orchestration
- OpenClaw on Mac Mini as core runtime and multi-agent router
- Agent definitions in versioned files (MISSION.md, agent configs)
- TOOLS.md describes tool capabilities and required approvals
- Task queue: sequential per-session to avoid race conditions

## Key Components

### Discord App
- Channel-based routing for each agent domain
- Event subscriptions for messages in specific channels

### Tool Layer (Muscles)
- HTTP client tools (CRM, email, web scrape, SEDAR+)
- CoreConX DB tools (read-only analytics, controlled writes)
- File/memory tools (logs, CSVs, docs)

### Persistence
- **App DB:** Supabase/Postgres for CoreConX data
- **Agent DB:** Task logs, approvals, research lists, contacts, support tickets
- **Agent memory:** OpenClaw file-based memory

## Data Access Model
- Read-only analytics via parameterized queries/stored procedures
- Controlled writes via defined functions (create_match_suggestion, add_customer_note, etc.)
- No freeform SQL from models
- All access logged and auditable

### Key Data Objects
- DrillerPerformanceSnapshot
- ProjectPerformanceSnapshot
- MatchSuggestion
- SupportTicket
- OutreachContact / OutreachSequence

## Research Pipeline (Mining Companies)
1. **Source discovery:** SEDAR+, company news, stock APIs for financing/drilling keywords
2. **Parsing:** Extract structured fields via muscle tasks
3. **Scoring:** Evaluate indicators (raised for drilling, meter counts, timing)
4. **Enrichment:** Company website, management team, contacts
5. **Output:** Target list with attributes, outreach timing/angle
6. **Feedback loop:** Dylan marks good/bad, agent learns scoring

## Guardrails
- Every tool has: risk level, allowed agents, approval channel (if level ≥1)
- Level 1-2 tools require plan/diff/preview posted to Discord + approval
- Full audit log: every tool call and result logged
