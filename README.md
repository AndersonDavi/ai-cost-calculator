# AI Cost Calculator

Compare the real cost and capacity of AI coding agents: **OpenCode Go**, **Claude Code**, and **Codex (ChatGPT)**.

**Live → [tools.andersonrueda.com](https://tools.andersonrueda.com)**

## What it does

- Estimates how many prompts/day you can realistically make on each subscription plan
- Models token window limits (not just price) for Claude Code and Codex
- Accounts for per-model quota multipliers (Opus costs more quota than Haiku, etc.)
- Shows costs in USD, COP, EUR, and MXN
- Exchange rates updated daily via GitHub Actions (zero external API calls for visitors)

## Presets

Five usage profiles calibrated to real agentic workloads (high cached-token ratios):

| Preset | Use case |
|--------|----------|
| Debug puntual | Isolated bug, short loops |
| Feature SaaS | Backend + frontend + flow review |
| Diseño / UX | Visual iteration, less heavy infra |
| Infra / DevOps | Logs, scripts, diagnostics |
| Seguridad / auditoría | High context, reasoning, traceability |

## Stack

Single HTML file — no build step, no dependencies beyond ECharts and Tabulator (loaded from CDN).

## Exchange rates

`rates.json` is updated once daily by a GitHub Action fetching from [frankfurter.dev](https://frankfurter.dev). Visitors never call any external rate API directly.

## License

MIT
