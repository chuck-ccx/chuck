# Sub-Agent Registry

| Agent | Purpose | Model Tier | Status | Spawned | Notes |
|-------|---------|------------|--------|---------|-------|
| mc-monitor | MC health checks: API, tunnel, tasks, email, CRM | T2 (Haiku) | Active | 2026-04-09 | Cron every 15min. Auto-restarts API server. Escalates to Chuck. |
| prospector | Nightly CRM prospecting: finds 5 new drilling companies | T2 (Haiku) | Active | 2026-04-09 | Cron nightly 10pm PDT. Adds to CRM sheet. Reports to Chuck. |
| outreach | Drafts personalized cold emails for new CRM leads | T3 (Sonnet) | Paused | 2026-04-09 | Cron nightly 10:30pm PDT. Hormozi-style value-first. Queues drafts for Dylan. |
| follow-up | Tracks leads, sends follow-ups at day 3/7/14 | T2 (Haiku) | Paused | 2026-04-09 | Cron daily 9am PDT. Escalates after 3 touches. |
| content | Drafts LinkedIn posts for Dylan's profile | T3 (Sonnet) | Paused | 2026-04-09 | Cron Mon/Wed/Fri 7am PDT. Industry insights + thought leadership. |
| support | First-line customer support for support@coreconx.group | T2 (Haiku) | Paused | 2026-04-09 | Cron every 30min 7am-6pm PDT. Auto-responds common Qs, escalates complex. |
| analytics | Weekly driller performance reports from Supabase | T2 (Haiku) | Paused | 2026-04-09 | Cron Monday 6am PDT. Google Docs reports for drill owners. |
| billing | Future marketplace invoicing ($1/meter) | T2 (Haiku) | Paused | 2026-04-09 | Placeholder — no schedule until marketplace launches. |

## Tiers
- **T1** (DeepSeek/Llama/Gemma) — routine, repetitive, low-judgment
- **T2** (Haiku/Flash) — structured, multi-step, needs some reasoning
- **T3** (Sonnet/Opus) — judgment, strategy, complex synthesis

## Rules
- All sub-agents report to Chuck, not Dylan
- Chuck QAs all sub-agent output before it reaches Dylan or external systems
- No sub-agent may send emails, post to Discord, or modify shared state directly
- Retire after 7 days idle
