# Glossary

| Term | Meaning |
|---|---|
| Hermes | The agent runtime installed at %LOCALAPPDATA%\hermes. |
| Gateway | Background service (Hermes_Gateway scheduled task) serving LLM requests and running cron jobs. |
| MCP | Model Context Protocol - the standard by which the agent calls external tool servers. |
| MCP server | A process (local or remote) exposing tools over MCP; declared under mcp_servers: in config.yaml. |
| Provider | An LLM backend declared under providers: in config.yaml. |
| IcyCache | Local LLM response cache / proxy on port 8080; exact match on the SHA256 of the normalised payload. |
| OmniRoute | Local multi-provider router on port 20128; 14 providers; comboStrategy=fallback. |
| LM Studio | Local inference server on port 1234; last-resort fallback, CPU-only on this machine. |
| Skill | A SKILL.md procedure file loaded into context on demand; 877 installed. |
| Plugin | A Python package under plugins/ that hooks into the agent runtime. |
| Cron job | A scheduled task defined in cron/jobs.json, executed by the gateway. |
| Session | A conversation record stored in state.db. |
| Redaction | Replacement of a secret value with [REDACTED]. |
