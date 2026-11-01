# Chapter 4 — Providers & Models

## 4.1 Provider Inventory

The Hermes installation on this host is configured with **19 providers** in `config.yaml`. Providers are listed in the `providers:` block starting at line 1107. Each provider entry specifies a `base_url`, an API key environment variable (`key_env`), and a `models` list.

| # | Provider | Base URL | Key Environment Variable | Notes |
|---|---------|----------|--------------------------|-------|
| 1 | icycache | `http://127.0.0.1:8080/v1` | `HERMES_ICYCACHE_API_KEY` | Local LLM response cache/proxy; routing entry point |
| 2 | cerebras | `https://api.cerebras.ai/v1` | `CEREBRAS_API_KEY` | Fast inference |
| 3 | cloudflare | `https://api.cloudflare.com/client/v4/accounts/[REDACTED]/ai/v1` | `CLOUDFLARE_API_TOKEN` | Workers AI |
| 4 | copilot | (GitHub Copilot endpoint) | `COPILOT_TOKEN` | GitHub Copilot |
| 5 | google-aistudio | `https://generativelanguage.googleapis.com/v1beta/openai/` | `GOOGLE_AI_STUDIO_API_KEY` | Google AI Studio |
| 6 | groq | `https://api.groq.com/openai/v1` | `GROQ_API_KEY` | Groq inference |
| 7 | huggingface-custom | `https://router.huggingface.co/v1` | `HF_CUSTOM_API_KEY` | Hugging Face router |
| 8 | kilo | `https://ai-gateway.kilo.ai/api/v1` | `KILO_API_KEY` | Kilo gateway |
| 9 | kilo-custom | `https://ai-gateway.kilo.ai/api/v1` | `KILO_CUSTOM_API_KEY` | Kilo custom endpoint |
| 10 | llm7io | `https://api.llm7.io/v1` | `LLM7_API_KEY` | LLM7 service |
| 11 | lmstudio | `http://127.0.0.1:1234/v1` | (none — local) | Last-resort fallback; CPU-only |
| 12 | mistral-custom | (Mistral endpoint) | `MISTRAL_CUSTOM_API_KEY` | Mistral |
| 13 | nous | (Nous Research endpoint) | `NOUS_API_KEY` | Nous Research free models |
| 14 | nvidia | (NVIDIA endpoint) | `NVIDIA_API_KEY` | NVIDIA NIM |
| 15 | ollama | `http://127.0.0.1:11434/v1` | (none — local) | Local Ollama |
| 16 | omniroute | `http://127.0.0.1:20128/v1` | `OMNIROUTE_API_KEY` | Local OmniRoute; 14 providers, comboStrategy=fallback |
| 17 | openrouter | `https://openrouter.ai/api/v1` | `OPENROUTER_API_KEY` | OpenRouter |
| 18 | openrouter-custom | `https://openrouter.ai/api/v1` | `OPENROUTER_CUSTOM_API_KEY` | OpenRouter custom |
| 19 | vertex | (Google Vertex AI) | `HERMES_CUSTOM_OMNIROUTE_API_KEY` | Default provider; project_id [REDACTED], region global |

> **Security note:** All API key values are redacted. Only the environment variable names are listed. No credential values appear in this document. See Annex B for the full redaction policy.

## 4.2 Model Routing Chain

The `model:` block (line 1024) defines the routing configuration:

- **API mode:** `chat_completions`
- **Default provider:** `vertex` (key_env: `HERMES_CUSTOM_OMNIROUTE_API_KEY`)
- **Switch context confirm tokens:** 50,000
- **Model aliases:** `qwen-max`, `qwen-plus`, `qwen-turbo`

The routing chain works as follows:

1. Requests first hit **IcyCache** (port 8080), which acts as the local LLM response cache/proxy.
2. IcyCache forwards to the configured upstream, defaulting to **OmniRoute** (port 20128).
3. OmniRoute (comboStrategy=fallback) tries its 14 providers in order.
4. If all cloud providers fail, the chain terminates at **LM Studio** (port 1234) as the last-resort local fallback (CPU-only inference).

```
Request → IcyCache (:8080) → OmniRoute (:20128) → [14 cloud providers, fallback]
                                                        ↓ (if all fail)
                                                   LM Studio (:1234, CPU-only)
```

## 4.3 Model Usage Statistics

The following statistics were extracted from `state.db` table `session_model_usage`. They represent the full lifetime of the installation.

### Aggregate Totals

| Metric | Value |
|--------|-------|
| Total sessions | 177 |
| Total messages | 58,242 |
| Total tokens (input + output + reasoning) | 287,216,338 |
| Input tokens | 276,999,011 |
| Output tokens | 9,383,612 |
| Reasoning tokens | 833,715 |

### Top 15 Models by API Call Count

| Rank | Model | API Calls | Input Tokens | Output Tokens |
|------|-------|-----------|--------------|---------------|
| 1 | nvidia/nemotron-3-ultra-550b-a55b | 336 | 48,195,701 | 4,129,066 |
| 2 | hermes-free | 67 | 56,467,896 | 976,770 |
| 3 | zai-org/glm-5.2-maas | 41 | 83,426,941 | (low) |
| 4 | meituan/longcat-2.5-preview:free | 26 | — | — |
| 5 | openai/gpt-oss-120b | 26 | — | — |
| 6 | deepseek-ai/deepseek-v4.1-flash-maas | 25 | — | — |
| 7 | gpt-4.1-2025-04-14 | 23 | — | — |
| 8 | nvidia/nemotron-3-ultra-550b-a55b:free | 18 | — | — |
| 9 | ornith/ornith-1.5-122b:free | 13 | — | — |
| 10 | reflection/beam-1.0-501b-a23b:free | 8 | — | — |
| 11 | google/gemini-4-argon | 7 | — | — |
| 12 | @cf/google/gemma-4-26b-a4b-it | 6 | — | — |
| 13 | upstage/solar-mini4:free | 4 | — | — |
| 14 | codestral-latest | 3 | — | — |
| 15 | google/gemini-3.8-flash | 3 | — | — |

### Top 10 Sessions by Token Consumption

| Rank | Session ID | Title | Model | Input Tokens |
|------|-----------|-------|-------|--------------|
| 1 | local-3d | Merge llm-spoofing-system into Roma project | glm-5.2-maas | 23,149,009 |
| 2 | local-34 | Redesign Neurole Core as Cosmic Icy Fire Cross Til | deepseek | 13,050,681 |
| 3 | local-ee | Ajouter scan proxy mondial temps réel | hermes-free | 10,910,216 |
| 4 | local-e2 | Use Icycache going forward | deepseek | 9,608,118 |
| 5 | local-81 | Fix Osiris launch without server and systray | deepseek | 8,585,024 |
| 6 | 20261022 | Debug Qdrant issue in French | longcat-2.5 | 8,421,898 |
| 7 | local-ad | Upload all skills from D:/Fichier SKILL | deepseek | 7,643,695 |
| 8 | local-7f | Exécuter job SA TA avec rapport par étape | deepseek | 7,570,982 |
| 9 | 20261021 | Hermes cron run 151ab57905e9 | hermes-free | 7,486,096 |
| 10 | local-50 | Indexer skills dans Qdrant et améliorer dashboard | deepseek | 7,083,474 |

## 4.4 Provider Configuration Details

### IcyCache (Port 8080)

IcyCache is the local LLM response cache and proxy. It intercepts model API calls, normalizes the request payload, and caches responses by SHA256 hash of the normalized payload. The cache is exact-match (no semantic matching). The upstream is selected by URL prefix, `X-IcyCache-Upstream` header, JSON body field, or the gateway default (`omniroute`).

Known issues:
- The `x-icycache-tokens-source` header returns `'?'` on cache hits.
- A 502 error after a 400 from upstream can block subsequent requests; a direct call to `:1234` resets it.

### OmniRoute (Port 20128)

OmniRoute is a local multi-provider router running 14 upstream providers with `comboStrategy=fallback`. The catalog contains 123 models. The `/api/settings` endpoint returns OK; `/api/routes` and `/api/settings/routing` return 404. The catalog does not include a `free` or `price` field, making programmatic free-tier filtering impossible.

### LM Studio (Port 1234)

LM Studio is the last-resort fallback provider, running on the local machine. All inference is CPU-only (the host has 0 VRAM on Intel Iris Xe). The embedding model `text-embedding-qwen3-embedding-4b` is loaded here for Qdrant vector operations. LM Studio should be preserved as the final fallback to avoid consuming RAM and CPU unnecessarily.

### Vertex

Vertex is the configured default provider. It uses the Google Cloud project with ID [REDACTED] in the `global` region. The API key is stored in the `HERMES_CUSTOM_OMNIROUTE_API_KEY` environment variable.
