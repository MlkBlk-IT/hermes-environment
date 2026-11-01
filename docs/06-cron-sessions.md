# Chapter 6 — Cron Jobs & Sessions

## 6.1 Cron Jobs

The Hermes installation runs **7 cron jobs** defined in `cron/jobs.json`. Six are enabled; one is disabled.

### Job Inventory

| # | Name | Schedule | Enabled | Completions | Last Status | Script/Agent |
|---|------|----------|---------|-------------|-------------|--------------|
| 1 | nous-expiry-watch | `0 9 * * *` (daily 9 AM) | Yes | 25 | ok | `ze_cron_expiry.py` |
| 2 | Morning briefing | `0 8 * * *` (daily 8 AM) | Yes | 27 | ok | Agent (no script) |
| 3 | Daily AI & Models Intelligence Brief | `0 7 * * *` (daily 7 AM) | Yes | 26 | ok | Agent (no script) |
| 4 | Health check | `*/5 * * * *` (every 5 min) | Yes | 493 | ok | `health_check.py` |
| 5 | Quota check | `0 * * * *` (hourly) | **No** | 9 | ok | `quota_check.py` |
| 6 | Daily summary | `0 8 * * *` (daily 8 AM) | Yes | 5 | ok | `daily_summary.py` |
| 7 | Check Nous free models daily | `0 9 * * *` (daily 9 AM) | Yes | 0 | (none) | Agent (no script) |

### Job Details

#### nous-expiry-watch (ID: 7cbcc5364bfc)
- **Purpose:** Monitors Nous Research free model availability and expiry.
- **Schedule:** Daily at 9:00 AM.
- **Last run:** 2026-10-31 09:01:00 EDT
- **Last failure:** 2026-10-27 09:00:47 — Script exited with code 1 (traceback in tools directory).
- **Model:** Unpinned (uses gateway default routing).

#### Morning briefing (ID: d5f12052e070)
- **Purpose:** Daily morning summary and task overview.
- **Schedule:** Daily at 8:00 AM.
- **Last run:** 2026-10-31 08:04:01 EDT
- **Last failure:** 2026-10-30 11:15:26 — RuntimeError: HTTP 404: Model 'gemini/gemini-3.7-flash' not found. (Model was removed from provider catalog.)

#### Daily AI & Models Intelligence Brief (ID: 151ab57905e9)
- **Purpose:** Daily intelligence report on AI and model developments.
- **Schedule:** Daily at 7:00 AM.
- **Last run:** 2026-10-31 07:06:31 EDT
- **Last failure:** 2026-10-28 08:08:43 — RuntimeError: No reply (all providers in fallback chain failed).

#### Health check (ID: 3b34a1d5f7a9)
- **Purpose:** System health monitoring (gateway, providers, MCP servers).
- **Schedule:** Every 5 minutes.
- **Last run:** 2026-10-31 20:35:58 EDT
- **Last failure:** 2026-10-31 18:15:57 — Script exited with code 1 (unhealthy status detected).
- **Completion count:** 493 (highest of all jobs, reflecting 5-minute frequency).

#### Quota check (ID: 5eb89ac61c24)
- **Purpose:** Tracks API quota usage across providers.
- **Schedule:** Hourly.
- **Status:** **Disabled.**
- **Last run:** 2026-10-26 20:00:30 EDT (before disabling).
- **Completions:** 9.

#### Daily summary (ID: 8aca73a726b2)
- **Purpose:** Daily activity summary.
- **Schedule:** Daily at 8:00 AM.
- **Last run:** 2026-10-31 08:00:59 EDT
- **Completions:** 5.

#### Check Nous free models daily (ID: 1e4cc03c0dfb)
- **Purpose:** Checks available free models from Nous Research.
- **Schedule:** Daily at 9:00 AM.
- **Last run:** None (never executed yet).
- **Completions:** 0.

### Cron Model Policy

All cron jobs use **unpinned model routing** — they use the gateway's default model routing (fallback hermes-free + OmniRoute) rather than being pinned to a specific model. This policy was adopted after observing that pinned models (e.g., `vertex/gemini`) fail when those models become unavailable, causing job failures. The unpinned approach ensures resilience through the fallback chain.

## 6.2 Sessions

### Overview

The `state.db` database contains **177 sessions** with **58,242 messages**. All sessions are active (archived = 0).

### Sessions by Source

| Source | Count | Percentage |
|--------|-------|------------|
| local | 99 | 55.9% |
| cron | 37 | 20.9% |
| oneshot | 20 | 11.3% |
| gui | 10 | 5.6% |
| telegram | 8 | 4.5% |
| desktop | 2 | 1.1% |
| cli | 1 | 0.6% |
| **Total** | **177** | **100%** |

### Token Consumption

| Metric | Value |
|--------|-------|
| Total tokens | 287,216,338 |
| Input tokens | 276,999,011 |
| Output tokens | 9,383,612 |
| Reasoning tokens | 833,715 |
| Average tokens/session | 1,622,805 |
| Average messages/session | 329 |

### Session Timeline

- **First session epoch:** 1791025206 (October 10, 2026)
- **Last session epoch:** 1793493449 (October 31, 2026)
- **Span:** 21 days of active use

### Session Storage

The `state.db` SQLite database is 419,528,704 bytes (~400 MB). It stores:
- `sessions` — Session metadata (source, timestamps, token counts, message counts)
- `messages` — Full message history with token counts
- `messages_fts` — Full-text search index over messages
- `session_model_usage` — Per-session, per-model token breakdown
- `system_prompts` — System prompt storage
- `gateway_heartbeats` — Gateway health check records
- `gateway_routing` — Gateway routing decisions
- `worker_executions` — Worker process execution records
- `async_delegations` — Async task delegation records
- `conversation_generations` — Generation metadata
- `delivery_obligations` — Message delivery tracking
- `session_admissions` — Session admission control
- `session_turn_leases` — Turn lease management
- `compression_locks` — Context compression locks
- `runtime_epoch` — Runtime epoch tracking
- `schema_version` — Database schema version
- `state_meta` — State metadata
- `hosted_room_*` — Hosted room tables (multiple)

## 6.3 Gateway Status

The Hermes gateway runs as a Windows Scheduled Task named `Hermes_Gateway`.

| Property | Value |
|----------|-------|
| Task name | Hermes_Gateway |
| Status | Ready |
| Process ID | 19324 |
| Last run time | 2026-10-31 20:17:24 |
| Last run result | 0 (success) |

The gateway is the local API endpoint that receives model requests from the Hermes agent and routes them through the provider chain. It manages rate limiting, fallback, and circuit breaker logic.
