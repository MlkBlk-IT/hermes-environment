# Chapter 5 — Skills & Plugins

## 5.1 Skills

### Overview

The Hermes installation contains **876 SKILL.md files** across the skills directory tree at `C:\Users\Tech\AppData\Local\hermes\skills`. Skills are organized into category subdirectories and provide specialized procedural knowledge that the agent loads on demand.

> **Note on count discrepancy:** The README and Glossary cite 877 skills. The `find` command returns 876 SKILL.md files. This 1-unit difference may result from a skill that shares both its name and path with another skill in the same directory tier, making it unresolvable by `skill_view`. The empirically verified count from filesystem traversal is **876**.

### Auto-Load Skills

Two skills are configured for automatic loading at session start via `skills.auto_load` in `config.yaml`:

| Skill | Purpose |
|-------|---------|
| `master-prompt` | Execution-mode contract; defines rules, workflow, speed constraints, security policy, and output format |
| `loom-context-cache` | Context caching via Loom MCP; mandates `ctx_store` for outputs >100 tokens and `ctx_search` before re-reading files |

### Skills Index

The skills index is compressed to approximately 22,000 tokens. Descriptions are truncated to ≤60 characters at sentence boundaries for the BM25 retrieval plugin. The index was optimized on 2026-10-31, reducing from 70,234 tokens (−69%).

Skills are categorized across the following top-level groups:

- `agent-operations` — Agent routing, orchestration, context caching, session management
- `autonomous-ai-agents` — Multi-provider routing, frontier model access, browser automation
- `business` — Lean startup, market research, negotiation
- `data-agent-kit` — BigQuery, GCP data pipelines, data engineering
- `devops` — Docker, Hermes config, MCP servers, Windows management
- `mlops` — Model deployment, local models, vector databases
- `security` — Active Directory, cloud security, forensics, malware analysis
- `software-development` — Code review, testing, MCP development, planning

### Skill Bundle Pattern

Skills can be loaded in groups via the `/bundle-name` pattern, which loads multiple skills in a single command. The `cold-skills-index` skill implements tiering between cold (archived) and warm (active) skills via `skills_archive/INDEX.md`.

## 5.2 Plugins

### Overview

The plugins directory at `C:\Users\Tech\AppData\Local\hermes\plugins` contains **28 entries** (27 directories + 1 archive). Of these, **26 plugins are active** and **12 are disabled** in `config.yaml`.

### Active Plugins

| # | Plugin | Description |
|---|--------|-------------|
| 1 | agent-ledger | Agent execution ledger |
| 2 | anysearch | Web search enhancement |
| 3 | approval-ledger | Approval request tracking |
| 4 | aux-ledger | Auxiliary execution ledger |
| 5 | context-audit | Context usage auditing |
| 6 | disk-cleanup | Disk space management |
| 7 | hermes-connector | Chrome browser bridge (ws://127.0.0.1:8767) |
| 8 | hermes-labyrinth | Complex workflow navigation |
| 9 | hermes-memory-ui | Memory management interface |
| 10 | hermes-newswire | News feed aggregation |
| 11 | hermes-plugin-guard | Plugin integrity guard |
| 12 | hermes-resetwatch | Reset event monitoring |
| 13 | hermes-rss | RSS feed management |
| 14 | hermes-security-audit | Security audit tooling |
| 15 | hermes-tailscale | Tailscale VPN integration |
| 16 | memory | Memory management |
| 17 | memory-wiki | Memory wiki |
| 18 | prompt-optimizer | Prompt optimization |
| 19 | quota | Quota tracking |
| 20 | qwen-chroma-auto | Automatic Qwen-Chroma memory |
| 21 | rea | Reverse engineering assistant |
| 22 | shodan | Shodan API integration |
| 23 | skill-integrity | Skill file integrity checking |
| 24 | skill-retrieval | BM25 skill retrieval (top-6 per turn) |
| 25 | snyk | Snyk vulnerability scanning |
| 26 | tokenwatch | Token usage monitoring |
| 27 | turn-usage | Turn-level usage tracking |

### Disabled Plugins

The following plugins are listed in the `disabled` section of `config.yaml`:

- `cron_providers/chronos`
- `hermes-memory-ui`
- `hermes-resetwatch`
- `hermes-rss`
- `hermes-tailscale`
- `memory-wiki`
- `prompt-optimizer`
- `security-guidance`
- `teams_pipeline`
- `turn-usage`

> **Note:** Some plugins appear in both enabled and disabled lists due to configuration overrides or conditional activation.

## 5.3 Skill-Plugin Interaction

The `skill-retrieval` plugin injects BM25 top-6 skill descriptions per turn, appended after the user's message. This allows the agent to discover relevant skills without scanning the full index. When a skill matches the task, the agent loads it via `skill_view(name)` to access its full content, including supporting files (references, templates, scripts).

The `qwen-chroma-auto` plugin provides automatic long-term memory through Qdrant vector storage. Relevant memories are recalled before turns, and durable information is extracted after completed turns. The embedding model runs on LM Studio (port 1234) using `text-embedding-qwen3-embedding-4b` (2560-dim vectors).

## 5.4 Skills Index Optimization (2026-10-31)

On 2026-10-31, the skills index underwent a comprehensive optimization:

- **Files processed:** 877 SKILL.md files
- **Descriptions truncated:** 694 (to ≤60 chars at sentence boundary)
- **Index tokens:** 70,234 → ~22,000 (−69%)
- **Full backup:** `skills.bak-trunc-20261031_135543` (138.7 MB)
- **Corrupted skills restored:** 7 ZIP-corrupted skills restored from `D:\Fichier SKILL\`
- **Validation:** All 877 files valid YAML, 0 corruption
