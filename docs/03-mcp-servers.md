# Chapter 3 - MCP Servers

## 3.1 What is declared

The mcp_servers block of config.yaml (starting at line 381) declares 85 configuration lines and
expands to 58 named servers across roughly 126 individual entries once nested keys are counted.

Each entry carries:

| Key | Meaning |
|---|---|
| command / url | How to launch or reach the server |
| args | Arguments passed to the launcher |
| env | Environment variables for the process |
| enabled | Whether the server is active |
| transport | stdio or http |

## 3.2 Full inventory

| # | MCP server | Notes |
|---|---|---|
| 1 | loom | Context cache (local drawer, MCP) |
| 2 | icycache | LLM response cache / proxy |
| 3 | agentic-tools | Agent helper toolset |
| 4 | agentled | Agent ledger |
| 5 | ai_stack_chrome_devtools | Chrome DevTools MCP |
| 6 | ai_stack_git | Git MCP (AI-Stack) |
| 7 | ai_stack_github | GitHub MCP (AI-Stack) |
| 8 | arxiv-mcp | arXiv paper search |
| 9 | aws-knowledge | AWS knowledge base |
| 10 | clirank | CLI ranking |
| 11 | cloudflare | Cloudflare API |
| 12 | codex-mcp | Codex tooling |
| 13 | context7 | Library documentation lookup |
| 14 | corpus-cloud-pricing | Cloud pricing corpus |
| 15 | dak-bigquery | Data Agent Kit - BigQuery |
| 16 | dak-gcs | Data Agent Kit - Cloud Storage |
| 17 | dak-notebook | Data Agent Kit - notebooks |
| 18 | dak-spanner | Data Agent Kit - Spanner |
| 19 | dak-viz | Data Agent Kit - visualization |
| 20 | darknet | Dark web research |
| 21 | chrome-mcp-server | Browser control |
| 22 | deepwiki | Repo wiki |
| 23 | desktopcommander | Desktop automation |
| 24 | duckduckgo-search | Web search |
| 25 | fetch | URL fetch + extraction |
| 26 | filesystem | Local file access |
| 27 | firecrawl | Web crawl / scrape |
| 28 | free-proxy | Proxy pool |
| 29 | free-search-mcp | Free search backend |
| 30 | gcloud | Google Cloud CLI |
| 31 | git | Git operations |
| 32 | git-mcp | Git MCP (alternate) |
| 33 | github | GitHub API |
| 34 | gitlab | GitLab API |
| 35 | globalping | Global network probes |
| 36 | hackernews | Hacker News |
| 37 | hugging_face | HF models / datasets |
| 38 | markitdown | Document to Markdown |
| 39 | mcp-linker | MCP orchestration |
| 40 | microsoft-learn | Microsoft Learn docs |
| 41 | mitre-attack | MITRE ATT&CK |
| 42 | ms365 | Microsoft 365 |
| 43 | neon | Neon Postgres |
| 44 | omniroute | Multi-provider router |
| 45 | parallel-search | Parallel search |
| 46 | playwright | Browser automation |
| 47 | postman | Postman API |
| 48 | pwsh | PowerShell execution |
| 49 | qwen-chroma | Vector memory (Qdrant + Qwen embeddings) |
| 50 | rea | Reverse engineering assistant |
| 51 | semgrep | Static analysis |
| 52 | sentry | Error monitoring |
| 53 | sequential-thinking | Reasoning scaffold |
| 54 | serena | Semantic code tooling |
| 55 | sqlite | SQLite access |
| 56 | supabase | Supabase |
| 57 | time | Time / timezone |
| 58 | todoist | Task manager |
| 59 | toragents | Tor agent tooling |
| 60 | torbrowser | Tor Browser control |

## 3.3 Categories

| Category | Servers |
|---|---|
| Context and memory | loom, icycache, qwen-chroma, context7, deepwiki |
| Search and web | fetch, firecrawl, duckduckgo-search, free-search-mcp, parallel-search, hackernews, arxiv-mcp |
| Dev and CI | git, git-mcp, github, gitlab, ai_stack_git, ai_stack_github, codex-mcp, semgrep, sentry, postman |
| Browser and desktop | playwright, chrome-mcp-server, ai_stack_chrome_devtools, desktopcommander, torbrowser, toragents |
| Cloud | gcloud, aws-knowledge, cloudflare, supabase, neon, dak-bigquery, dak-gcs, dak-spanner |
| Microsoft | microsoft-learn, ms365, pwsh |
| Security and RE | mitre-attack, darknet, rea, free-proxy, globalping |
| System and files | filesystem, sqlite, time, markitdown |
| Misc | hugging_face, agentled, agentic-tools, clirank, corpus-cloud-pricing, mcp-linker, sequential-thinking, serena, omniroute, todoist |

## 3.4 Locally hosted servers

The servers that run on this machine rather than a remote backend:

| Server | Backing service |
|---|---|
| loom | D:\Loom (MCP context cache) |
| icycache | IcyCache on port 8080 |
| omniroute | OmniRoute on port 20128 |
| qwen-chroma | Qdrant on port 6333 + LM Studio on port 1234 |
| pwsh | PowerShell 7 |
| filesystem, sqlite, markitdown, time | Native |
