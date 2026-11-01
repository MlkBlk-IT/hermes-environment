# Chapter 2 - Configuration

## 2.1 config.yaml at a glance

| Property | Value |
|---|---|
| Path | C:\Users\Tech\AppData\Local\hermes\config.yaml |
| Size | 117,592 bytes |
| Lines | 3,318 |
| Schema version | _config_version: 51 |
| Top-level sections | 31 |

## 2.2 Top-level sections

The configuration is organised into 31 top-level keys. The sections that carry the most weight:

| Section | Approx. line | Content |
|---|---|---|
| _config_version | 1 | Schema version (51) |
| agent | ~40 | Agent behaviour, approvals, auxiliary models |
| model | 1024 | Default model, fallback chain |
| plugins | 1068 | Plugin enablement and configuration |
| providers | 1107 | 19 LLM providers |
| mcp | ~360 | MCP-related global settings |
| mcp_servers | 381 | MCP server declarations (85 lines in block) |
| security | ~3200 | Approval, redaction, private URLs |
| skills | 3284 | Skill system settings and auto-load list |

## 2.3 Agent and approvals

The agent section defines fallback chains and approval behaviour. Notable settings observed:

| Setting | Value |
|---|---|
| write_approval | false |
| inline_shell | false |
| guard_agent_created | true |

Approval behaviour is defined per tool class; the configuration keeps write approval disabled for
agents the system itself created, while guarding against unvetted agent-created content.

## 2.4 Security settings

| Setting | Value |
|---|---|
| allow_private_urls | true |
| protected_instruction_files | true |
| redact_secrets | *** |
| telemetry shared_metrics | enabled: false / send: false |
| offer_version | 2 |

Interpretation:

- allow_private_urls: true lets the agent reach local services (IcyCache, OmniRoute, LM Studio).
- protected_instruction_files: true prevents external content from overriding SOUL.md and friends.
- redact_secrets: *** masks secret values in logs and output.
- Telemetry is off: nothing is sent out.

## 2.5 Model and routing settings

The model section (line 1024) holds the default model and the fallback chain used when a provider
fails. The chain terminates at LM Studio (local, CPU) as the last resort, consistent with the
zero-cost and resource-preservation constraints.

The skills section (line 3284) declares the auto-load list. Two skills are loaded in every session:

- master-prompt
- loom-context-cache

These are injected at session start and define the execution contract and the context-caching behaviour.
