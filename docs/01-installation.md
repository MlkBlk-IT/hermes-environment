# Chapter 1 - Installation and Environment

## 1.1 Host hardware and OS

| Item | Value |
|---|---|
| Machine | Dell Latitude 5420 |
| CPU | Intel Core i7-1185G7 |
| RAM | 32 GB |
| GPU | Intel Iris Xe, 0 GB dedicated VRAM |
| Inference | CPU only, no CUDA |
| OS | Windows 11 build 26300 |
| Shells | PowerShell 7, Git-Bash (MSYS) |
| User | Tech |
| Timezone | America/Montreal (EDT, UTC-4) |

Because there is no GPU, all local inference runs on CPU. LM Studio is therefore configured as the last fallback in the routing chain, to preserve RAM and CPU for interactive work.

## 1.2 Hermes root layout

Root: C:\Users\Tech\AppData\Local\hermes

| Path | Role |
|---|---|
| config.yaml | Main configuration - 3,318 lines, 117,592 bytes, _config_version 51 |
| state.db | Sessions, projects, shared state, verification evidence, kanban - 419,528,704 bytes |
| projects.db, shared-state.db, verification_evidence.db, kanban.db | Auxiliary stores |
| .env | Secrets file, 27,606 bytes - contents not reproduced |
| auth.json | Authentication material - contents not reproduced |
| memories/ | MEMORY.md (persistent agent notes), USER.md (user profile) |
| skills/ | 877 SKILL.md files |
| plugins/ | 27 directories, 26 active plugins |
| cron/jobs.json | Scheduled job definitions |
| backups/ | Timestamped configuration backups |
| bin/ | Executables |
| cache/, audio_cache/, bootstrap-cache/ | Runtime caches |
| browser-profile/ | Browser automation profile |
| attachments/, composer-pastes/, assets/, archives/ | Content stores |

## 1.3 Binaries

| Binary | Role |
|---|---|
| hermes.exe | Main CLI (gateway control, sessions) |
| hermes.cmd | Wrapper |
| hermes-acp.exe | ACP entry point |
| hermes-acp.cmd | Wrapper |
| uv.exe, uvw.exe, uvx.exe | Python toolchain (uv) |
| geckodriver.exe | WebDriver for browser automation |

## 1.4 Gateway service

Command: hermes.exe gateway status

| Field | Value |
|---|---|
| Scheduled task | Hermes_Gateway - Ready |
| PID | 7692 |
| Last run | 2026-10-31 19:27:46 |
| Result | 0 (success) |

The gateway owns model routing and cron execution, and refuses a restart while a session is active.
