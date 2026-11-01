# Annex C — Personal Notes & Configuration Rationale

## C.1 User Profile

- **Identity:** Malik, Montreal, student in DEP informatique at EMICA (graduation October 2027)
- **Role:** Freelance IT/Cloud Security
- **GitHub:** mlkblk-it
- **Skill level:** Autodidact, advanced — prefers direct technical details, no simplification

## C.2 Hardware Constraints

The host machine is a Dell Latitude 5420 with an Intel i7-1185G7 processor and 32 GB RAM. The integrated Intel Iris Xe GPU has 0 VRAM, meaning all local inference is CPU-only. This constrains the choice of local models and makes LM Studio the last-resort fallback to preserve system resources.

## C.3 Budget Constraint

The user operates on a $0 budget — only free-tier models are used. No paid models are ever used. This constraint drives the multi-provider fallback architecture: when one free provider's quota is exhausted or the model is removed, the chain falls back to the next available free provider, ultimately terminating at LM Studio (local, free).

## C.4 Configuration Decisions

### Why Unpinned Cron Models
Cron jobs use unpinned model routing (gateway default) rather than being pinned to a specific model. This decision was made after observing that pinned models (e.g., `vertex/gemini`) fail when those models become unavailable, causing job failures. The unpinned approach ensures resilience through the fallback chain.

### Why IcyCache as Entry Point
IcyCache sits at the entry of the model routing chain to cache LLM responses by SHA256 hash. This reduces API calls to cloud providers, saving quota and latency. The user's goal is maximizing cache hits and minimizing misses.

### Why LM Studio Last
LM Studio is CPU-only and consumes RAM. Placing it last in the fallback chain ensures it is only used when all cloud providers fail, preserving system resources for the agent's primary workload.

### Why Context Caching (Loom)
Loom provides lossless context storage with semantic search, achieving 94-98% token economy on large outputs. This allows the agent to offload context from the prompt window to local storage, reducing token consumption and cost.

### Why Dual Indexing (Loom + Qdrant)
The user's "INDEXE TOUT" directive requires dual indexing: Loom for exact context retrieval (chunk_id references) and Qdrant for semantic vector search. Both systems use the same source text, with metadata linking Loom chunk_ids to Qdrant entries for bidirectional traceability.

## C.5 Known Issues & Workarounds

### curl_cffi on Python 3.14+
`curl_cffi` lacks a `_cffi_backend` wheel on Python 3.14+. Must use `py -3.12` explicitly. Venv creation: `py -3.12 -m venv .venv`.

### Hermes Connector Port Mismatch
The Chrome extension points to port 18767 by default, but the Hermes Connector broker runs on port 8767. This mismatch causes handshake failures. Correction requires manual changes in the Chrome sidebar (not programmatically modifiable).

### LM Studio MCP Reload
LM Studio only reloads `mcp.json` at startup. After patching an MCP server configuration, LM Studio must be restarted for changes to take effect.

### Qdrant Embedding Dimension Mismatch
The `find_embedding_model()` function in `hermes_qdrant_mcp.py` is patched to prefer "qwen3" models. Without this patch, a 384-dim model is selected, causing a dimension mismatch with the 2560-dim Qdrant collection.

### IcyCache 502 Block
After a 400 error from an upstream provider, IcyCache can enter a state where it returns 502 for subsequent requests. A direct call to `:1234` (LM Studio) resets this state.

## C.6 User Communication Style

The user communicates literally and directly. Caps and profanity signal frustration (not aggression). The agent must:
- Execute exactly what is asked, word for word
- No initiative, no interpretation, no "helpful" extras
- Direct questions get one-sentence answers
- No unsolicited opinions or recommendations
- Verbosity is a failure signal

## C.7 Agent Operating Principles

The agent operates under the following standing principles (from `master-prompt` skill):

1. **Truth > confidence** — Never invent commands, flags, paths, outputs, or capabilities.
2. **Evidence > assumption** — Verify claims via docs, inspection, source, or execution.
3. **Execution > discussion** — If authorized and tools exist, act immediately.
4. **Safety > speed** — Protect data, credentials, availability, and rollback paths.
5. **No theater** — No fake progress, hype, filler, or unsupported success claims.

The workflow follows: Clarify → Inspect (read-only) → Decide → Execute (authorized) → Verify → Report.
