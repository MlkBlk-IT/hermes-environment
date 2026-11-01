# Chapter 7 — Projects & Ecosystem

## 7.1 D: Drive Project Inventory

The `D:\` drive hosts the user's project ecosystem. The following top-level directories were identified at the time of inspection:

| # | Directory | Description |
|---|-----------|-------------|
| 1 | AI | AI-related tools and models |
| 2 | AI-Stack | AI stack configuration |
| 3 | AURORA | Aurora project |
| 4 | awesome-mcp-servers | MCP server collection |
| 5 | Backups | System backups |
| 6 | CacheForge | Cache tooling project |
| 7 | Docu-Hermes | This documentation project |
| 8 | Fichier SKILL | Skill file repository (source for skill restoration) |
| 9 | FocusDopa-Android | Android focus application |
| 10 | freellmapi | Free LLM API project |
| 11 | GitHub-Promo | GitHub promotional materials |
| 12 | Hermes | Hermes installation/clone |
| 13 | IcyCache | LLM response cache (port 8080) |
| 14 | Icy-Loom | Loom/IcyCache integration |
| 15 | Lab-SRV1 | Lab server 1 |
| 16 | LoFT | LoFT project |
| 17 | LoFT-Git | LoFT Git repository |
| 18 | Loom | Context cache MCP server |
| 19 | MCP | MCP server collection |
| 20 | MCP_TEST | MCP testing |
| 21 | MCP-Osiris | Osiris MCP server |
| 22 | MlkBlk-IT | Personal IT brand |
| 23 | OpenClaw | OpenClaw project |
| 24 | projects | General projects |
| 25 | secrets | Secrets store (access restricted) |
| 26 | SkillClaw | Skill-related project |
| 27 | skills-repos | Skills repositories |
| 28 | Tiny11Builder-0.1.3-win-x64 | Tiny11 Windows builder |
| 29 | via-roma-reserve | ViaRoma reserve |
| 30 | VM | Virtual machines |

## 7.2 Core Infrastructure Projects

### IcyCache (Port 8080)

IcyCache is a local LLM response cache and proxy developed by the user. It sits between the Hermes agent and upstream model providers, caching responses by SHA256 hash of the normalized request payload. The cache uses exact matching (no semantic similarity).

**Key characteristics:**
- Upstream selection: URL prefix, `X-IcyCache-Upstream` header, JSON body field, or gateway default
- Default upstream: OmniRoute (port 20128)
- Cache key: SHA256 of normalized payload
- Known issue: `x-icycache-tokens-source='?'` on cache hits
- Known issue: 502 blocks after a 400 from upstream; direct `:1234` call resets

The user's goal for IcyCache: more cache hits, fewer misses.

### OmniRoute (Port 20128)

OmniRoute is a local multi-provider router that aggregates 14 upstream cloud providers with `comboStrategy=fallback`. It provides 123 models through its catalog.

**Key characteristics:**
- 14 providers in fallback chain
- 123 models in catalog
- `comboStrategy=fallback`
- API endpoints: `/api/settings` (OK), `/api/routes` (404), `/api/settings/routing` (404)
- No `free` or `price` field in catalog → programmatic free-tier filtering impossible

### Loom (Context Cache)

Loom is an MCP context cache server that provides lossless context storage with semantic search. It performs anchor compression with embeddings (semantic vector + FTS5), achieving 94-98% token economy on large outputs.

**Key characteristics:**
- `ctx_store` = vector indexing (embeddings + FTS5), not just exact storage
- `ctx_search` = semantic search (cross-language: FR finds EN), falls back to FTS
- `ctx_get` = byte-identical retrieval of stored chunks
- `ctx_transfer` = cross-session chunk transfer
- Saved ratio can be negative (anchor may use more tokens than original — expected, preserves density)

### LM Studio (Port 1234)

LM Studio is the last-resort local inference provider. All inference is CPU-only (Intel Iris Xe, 0 VRAM). The embedding model `text-embedding-qwen3-embedding-4b` is loaded here for Qdrant vector operations.

**Key characteristics:**
- CPU-only inference (no GPU)
- Used for embeddings (Qdrant) and last-resort text generation
- Should be preserved (avoid unnecessary RAM/CPU consumption)
- MCP servers are loaded at startup; LM Studio must be restarted after MCP config changes

## 7.3 Qdrant Vector Database

Qdrant collection `hermes_qwen3_memory` runs at `http://127.0.0.1:6333`. It stores 2560-dimensional embeddings from the `text-embedding-qwen3-embedding-4b` model served by LM Studio.

**Key characteristics:**
- Collection: `hermes_qwen3_memory`
- Embedding dimensions: 2560
- Embedding model: `text-embedding-qwen3-embedding-4b` (via LM Studio :1234)
- MCP server: `qwen-chroma` at `C:\AI\EmbeddingDB\hermes_qdrant_mcp.py`
- The `find_embedding_model()` function is patched to prefer "qwen3" (avoids 384-dim mismatch)

## 7.4 User-Created Tools

The user has built several tools that directly enable agent capabilities:

| Tool | Purpose |
|------|---------|
| IcyCache | LLM response caching (reduces API calls, saves tokens) |
| Loom | Context cache with semantic search (offloads context from prompt) |
| Osiris | Task engine with keyword registry |
| hunter.py | Local AI detection (`detect_local_ai`) |
| ViaRoma | Full AI gateway: FastAPI, MCP server, 16 provider adapters, circuit breaker, egress pool, identity binding, TLS fingerprinting, Three.js/WebGL dashboard, Colisée 3D |

ViaRoma was built solo in approximately 16 hours, demonstrating exceptional engineering velocity.
