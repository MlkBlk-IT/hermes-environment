# Annex B — Redacted Secrets Policy

## B.1 Policy Statement

This document adheres to a strict redaction policy. **No credential values appear anywhere in this documentation.** Every API key, token, secret, connection string, and sensitive identifier is replaced by `[REDACTED]`.

## B.2 Redacted Items

The following items have been identified in the Hermes installation and are redacted in this document:

### API Key Environment Variables

| Variable Name | Provider | Status |
|---------------|----------|--------|
| HERMES_ICYCACHE_API_KEY | icycache | [REDACTED] |
| CEREBRAS_API_KEY | cerebras | [REDACTED] |
| CLOUDFLARE_API_TOKEN | cloudflare | [REDACTED] |
| COPILOT_TOKEN | copilot | [REDACTED] |
| GOOGLE_AI_STUDIO_API_KEY | google-aistudio | [REDACTED] |
| GROQ_API_KEY | groq | [REDACTED] |
| HF_CUSTOM_API_KEY | huggingface-custom | [REDACTED] |
| KILO_API_KEY | kilo | [REDACTED] |
| KILO_CUSTOM_API_KEY | kilo-custom | [REDACTED] |
| LLM7_API_KEY | llm7io | [REDACTED] |
| MISTRAL_CUSTOM_API_KEY | mistral-custom | [REDACTED] |
| NOUS_API_KEY | nous | [REDACTED] |
| NVIDIA_API_KEY | nvidia | [REDACTED] |
| OMNIROUTE_API_KEY | omniroute | [REDACTED] |
| OPENROUTER_API_KEY | openrouter | [REDACTED] |
| OPENROUTER_CUSTOM_API_KEY | openrouter-custom | [REDACTED] |
| HERMES_CUSTOM_OMNIROUTE_API_KEY | vertex (default) | [REDACTED] |

### Project IDs

| Item | Status |
|------|--------|
| Google Cloud project_id (vertex) | [REDACTED] |
| Cloudflare account ID | [REDACTED] |

### Local Service Tokens

| Item | Status |
|------|--------|
| OmniRoute API key | [REDACTED] |
| IcyCache API key | [REDACTED] |
| Qdrant API key (if configured) | [REDACTED] |

## B.3 What Is NOT Redacted

The following items are not credentials and are preserved in full:

- Provider names and base URLs (public endpoints)
- Port numbers for local services (8080, 20128, 1234, 6333, 8767)
- Model names and identifiers
- Configuration keys and section names
- File paths and directory structures
- Cron job schedules and names
- Session counts, message counts, and token statistics
- Plugin and skill names
- MCP server names and categories

## B.4 Verification

All redaction was verified by searching the final document text for patterns matching common credential formats:
- `sk-` prefixes
- `Bearer` tokens
- Hexadecimal strings longer than 20 characters
- Base64-encoded strings longer than 40 characters
- `password=` or `key=` or `token=` assignments

No credential values were found in the final output.
