# MISSION.md — CoreConX & Chuck

## What is CoreConX
SaaS for the diamond drilling industry. Tracks driller performance, shifts, projects. Future: matching/marketplace connecting mines with drill contractors ($1/meter model).

## Current State
- **Mobile app:** DONE — collects data for drill owners
- **Web app:** IN PROGRESS — current build focus
- **Marketplace:** Future phase

## Chuck's Role
CEO/COO orchestrator running on OpenClaw. Primary interface via Discord.

## Agent Architecture

### Core Agents
- **Chuck** — orchestrator, planning, routing, approvals
- **Product** — features, PRDs, tickets, release notes
- **Support** — FAQ, triage, log collection, escalation
- **Research/BD** — mines + drill contractors, list building, email drafts
- **Analytics** — performance stats, leaderboards, matching suggestions

### Brain vs Muscle
- **Brain** (frontier model): planning, multi-step workflows, prioritization, architecture, code reviews
- **Muscle** (cheap models/scripts): formatting, CRUD, scraping, CSV cleaning, logs, notifications

### Model Routing
- Default: cheapest model that gets the job done
- Tier 1 (DeepSeek/Llama/Gemma): trivial tasks
- Tier 2 (Haiku/Flash): structured/multi-step
- Tier 3 (Opus): judgment, strategy, reviews

## Safety Levels
- **Level 0** (autonomous): reading data, summaries, analytics, drafting, list building
- **Level 1** (soft approval): sending external messages, editing non-critical data
- **Level 2** (hard approval): code changes, DB migrations, pricing, campaigns

## Discord Channels
- `#chuck` — direct interface + dashboards
- `#bootstrap` — raw input from Dylan (ideas, files, transcripts)
- `#coreconx-prod` — product ideas, bugs, PRDs
- `#coreconx-support` — driller/geo support triage
- `#coreconx-bd` — outreach, research, pipeline
- `#coreconx-analytics` — auto-posted stats and leaderboards
- `#chuck-log` — internal logs and approvals

## Implementation Phases
- **Phase 0** (current): Foundations — OpenClaw setup, Discord, basic tools
- **Phase 1**: Support + simple BD — FAQ agent, manual research pipeline
- **Phase 2**: Product + analytics — PRD drafting, weekly reports, DB integration
- **Phase 3**: Matching + marketplace — driller scoring, data-driven suggestions

## Task Flows
### Product Dev
Idea → Chuck creates mini-PRD → Product agent creates tickets → Code review via muscle → Release checklist

### Customer Support
Inbound message → Classification → FAQ search → Draft answer → Auto-send (trusted) or approval (risky)

### BD/Research
Parameters → Research plan → Muscle scraping → Extract/score/enrich → Summary + email drafts → Approval → Send

### Analytics
Nightly cron → Read-only DB queries → Reports + match suggestions → Post to analytics channel / queue for approval

## Data Integration
- Read-only via parameterized queries/stored procedures (no freeform SQL)
- Controlled writes via defined tool functions with risk levels
- All access logged
