# Preface

## Why this document exists

This Hermes installation has grown into a substantial system: 877 skills, 85 MCP server entries,
19 model providers, 26 active plugins, 176 sessions and more than 230 million tokens consumed.
None of that was documented in one place. This document is that single place. It records what is
installed, how it is configured, how traffic is routed, what runs automatically, and what is broken.

## Scope

In scope: the installation layout under %LOCALAPPDATA%\hermes; config.yaml in full; MCP servers;
model providers and the three-layer routing chain; skills and plugins; cron jobs and their run
history; sessions and token accounting; the external projects on D: that build on Hermes.

Out of scope: credential values (redacted by policy); third-party services unrelated to Hermes;
any modification of the running system.

## Method

Everything here came from read-only inspection:

| Source | What it gave |
|---|---|
| config.yaml (3,318 lines, v51) | MCP servers, providers, plugins, skills, model, security |
| cron/jobs.json | Cron job definitions and run history |
| state.db (read-only SQL) | Sessions, model usage, token totals |
| plugins/, skills/, bin/ | Filesystem inventory |
| Local HTTP probes | IcyCache and gateway health |
| hermes.exe gateway status | Gateway process state |

## Conventions

- Prose in English; commands, paths and identifiers in their native form.
- Figures are measurements, taken once, with the command shown.
- Anything uncertain is labelled as such rather than presented as fact.
- [REDACTED] marks a removed secret.
