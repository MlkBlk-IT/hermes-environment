# Annex A — Full Inventory

## A.1 Configuration File

| Property | Value |
|----------|-------|
| Path | `C:\Users\Tech\AppData\Local\hermes\config.yaml` |
| Lines | 3,318 |
| Size | 117,592 bytes |
| Config version | 51 |
| Top-level sections | 31 |

### Key Section Line Offsets

| Section | Line |
|---------|------|
| `mcp_servers` | 383 |
| `model` | 1024 |
| `plugins` | 1068 |
| `providers` | 1107 |
| `skills` | 3284 |

## A.2 Providers (19)

1. icycache
2. cerebras
3. cloudflare
4. copilot
5. google-aistudio
6. groq
7. huggingface-custom
8. kilo
9. kilo-custom
10. llm7io
11. lmstudio
12. mistral-custom
13. nous
14. nvidia
15. ollama
16. omniroute
17. openrouter
18. openrouter-custom
19. vertex

## A.3 MCP Servers (58 named)

The `mcp_servers` block in `config.yaml` spans 85 config lines and defines 58 named servers across approximately 126 entries. See Chapter 3 for the full server inventory and categorization.

**Local servers:** loom, icycache, omniroute, qwen-chroma, pwsh, filesystem, sqlite, markitdown, time

## A.4 Plugins

### Active (26+)

agent-ledger, anysearch, approval-ledger, aux-ledger, context-audit, disk-cleanup, hermes-connector, hermes-labyrinth, hermes-memory-ui, hermes-newswire, hermes-plugin-guard, hermes-resetwatch, hermes-rss, hermes-security-audit, hermes-tailscale, memory, memory-wiki, prompt-optimizer, quota, qwen-chroma-auto, rea, shodan, skill-integrity, skill-retrieval, snyk, tokenwatch, turn-usage

### Disabled (12)

cron_providers/chronos, hermes-memory-ui, hermes-resetwatch, hermes-rss, hermes-tailscale, memory-wiki, prompt-optimizer, security-guidance, teams_pipeline, turn-usage

## A.5 Cron Jobs (7)

| # | Name | ID (short) | Schedule | Enabled | Completions |
|---|------|-----------|----------|---------|-------------|
| 1 | nous-expiry-watch | 7cbcc5364bfc | 0 9 * * * | Yes | 25 |
| 2 | Morning briefing | d5f12052e070 | 0 8 * * * | Yes | 27 |
| 3 | Daily AI & Models Intelligence Brief | 151ab57905e9 | 0 7 * * * | Yes | 26 |
| 4 | Health check | 3b34a1d5f7a9 | */5 * * * * | Yes | 493 |
| 5 | Quota check | 5eb89ac61c24 | 0 * * * * | No | 9 |
| 6 | Daily summary | 8aca73a726b2 | 0 8 * * * | Yes | 5 |
| 7 | Check Nous free models daily | 1e4cc03c0dfb | 0 9 * * * | Yes | 0 |

## A.6 Skills

| Property | Value |
|----------|-------|
| Total SKILL.md files | 876 |
| Skills directory | `C:\Users\Tech\AppData\Local\hermes\skills` |
| Auto-load skills | master-prompt, loom-context-cache |
| Index tokens (optimized) | ~22,000 |
| Index optimization date | 2026-10-31 |

## A.7 Sessions

| Property | Value |
|----------|-------|
| Total sessions | 177 |
| Total messages | 58,242 |
| Total tokens | 287,216,338 |
| Archived sessions | 0 |
| state.db size | 419,528,704 bytes (~400 MB) |

## A.8 Gateway

| Property | Value |
|----------|-------|
| Scheduled task | Hermes_Gateway |
| Status | Ready |
| PID | 19324 |
| Last run | 2026-10-31 20:17:24 |
| Last result | 0 (success) |

## A.9 Model Routing

| Property | Value |
|----------|-------|
| API mode | chat_completions |
| Default provider | vertex |
| Key env | HERMES_CUSTOM_OMNIROUTE_API_KEY |
| Switch context confirm tokens | 50,000 |
| Model aliases | qwen-max, qwen-plus, qwen-turbo |
| Last-resort fallback | lmstudio (port 1234, CPU-only) |

## A.10 Agent Settings

| Property | Value |
|----------|-------|
| Mode | smart |
| Max turns | 40 |
| Reasoning effort | low |
| Timeout | 500 |
| Write approval | false |
| Inline shell | false |
| Guard agent created | true |
| Destructive slash confirm | false |
| Background review | (enabled) |
| Idle compact after seconds | 1800 |
| Proactive prune tokens | 0 |
| Cloud provider | nous |
| Restrict evaluate | true |
| Use real profile | true |

## A.11 Security Settings

| Property | Value |
|----------|-------|
| Allow private URLs | true |
| Protected instruction files | true |
| Redact secrets | *** |
