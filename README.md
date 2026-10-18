# AI Cost Calculator

Compare the real cost and capacity of AI coding agents: **OpenCode Go**, **Claude Code**, and **Codex (ChatGPT)**.

**Live → [tools.andersonrueda.com](https://tools.andersonrueda.com)**

## What it does

- Estimates how many prompts/day you can realistically make on each subscription plan
- Models token window limits (not just price) for Claude Code and Codex
- Models OpenCode Go's per-model dollar caps (5h window = 20%, week = 50% of the monthly cap) for both Go ($10) and Go Plus ($40)
- Accounts for per-model quota multipliers (Opus costs more quota than Haiku, etc.)
- Applies long-context pricing tiers (e.g. >272K input on GPT-6, >100K on Haiku 5.5)
- Shows API input/output prices per million tokens directly in every model selector
- Covers Claude Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1, and OpenAI's GPT-6 family (Astra, Sol, Luna) plus GPT-6.1 Sol, with coding benchmarks
- Documents the unified ChatGPT desktop app with Chat, Work, and Codex on macOS and Windows
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
