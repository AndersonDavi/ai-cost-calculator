# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

AI Cost Calculator: compares cost and real capacity (prompts/day, token windows) of OpenCode Go, Claude Code and Codex (ChatGPT) subscription plans. Live at tools.andersonrueda.com (GitHub Pages, see `CNAME`). UI text and comments are in Spanish.

## Commands

There is no build, lint or test tooling. It is a single static page.

- Run locally: `npx serve -p 5500 .` (also configured as the `ai-cost-calculator` server in `.claude/launch.json`, usable via `preview_start`).
- Verify changes by loading the page in the browser and checking the console; there is no automated test to run.

## Architecture

Everything lives in [index.html](index.html) (~3600 lines): CSS, markup, and one large inline `<script>` starting around line 1740. ECharts and Tabulator are loaded from CDNs; there are no modules or dependencies to install.

The script is organized as data tables followed by render/calc functions:

- **Data tables (top of script):** `presets`, `usageProfiles`, `goModels`, `claudeModels`, `codexModels`, `claudePlanWindowTokens`, `codexPlanWindowTokens`, `benchmarkModels`, `benchmarkScenarios`, `timelineEvents`, `claudeFeatureData`/`codexFeatureData`. Adding or updating a model/price/plan mostly means editing these objects (e.g. a new Claude model needs `input`, `cached`, `output`, `quotaMultiplier`; Go models need `monthlyLimit`, `plusMonthlyLimit` and `req5h/reqWeek/reqMonth` from opencode.ai/docs/go, and `costPerReq = monthlyLimit / reqMonth` is derived). Optional `longContext: { threshold, input, cached, output }` switches rates when input + cached tokens exceed the threshold (`pricingFor`). `apiPriceNote` is display-only.
- **OpenCode Go limits are per model:** the 5h window is `GO_WINDOW_SHARE` (20%) and the week `GO_WEEK_SHARE` (50%) of each model's monthly dollar cap. `buildGoPlans` builds Go $10 and Go Plus $40 rows for the same multi-plan table Claude/Codex use.
- When the catalog changes, also update presets' default models, the per-model status texts in `calculate()` (`claudeStatusByModel` / `codexStatusByModel` must have a key for every model), `timelineEvents`, `benchmarkModels`, the README feature list and the "Actualizado" date in the hero.
- **Central `state` object** holds the selected preset, usage profile, per-provider model, currency and FX rates. Preset selection (`applyPreset`) also sets the default model for each provider.
- **Calculation flow:** `getInputs` → `calculateDesiredUsage` → `buildGoPlans` / `buildClaudePlans` / `buildCodexPlans` → `calculatePlanCapacity` → `renderMultiCapacity`. `calculate()` orchestrates everything and re-renders all cards; call it after any state change. Claude/Codex are modeled on token *window* limits and per-model quota multipliers, not only price.
- **Benchmark section:** `initBenchmark` builds the price/benchmark Tabulator table and ECharts scatter chart from `benchmarkModels`.
- `findSuggestedPlan` matches plans by name and amount from the provider's hint string (`detectPlanBudget`); new hint strings must be added to that map.
- Top-level `const`s live in the classic script's lexical scope, not on `window`, so they aren't reachable from browser devtools/automation as `window.x`; drive the UI through `[data-preset]` / `[data-model]` clicks (re-query after each click, the buttons are re-rendered).
- `index.html` uses CRLF line endings.

## Exchange rates

Visitors never call a rate API directly. `.github/workflows/update-rates.yml` runs daily, fetches frankfurter.dev and commits `rates.json` to the **`data` branch** (not `main`). `fetchLiveRates()` reads it from `raw.githubusercontent.com/.../data/rates.json`, falling back to local `/rates.json`, then to frankfurter.dev directly; results are cached in `localStorage` (`ai-trm`, `ai-currency`). The `rates.json` on `main` is only a dev fallback and can be stale.

## Conventions

Commits use conventional prefixes (`feat:`, `fix:`, `chore:`) with Spanish or English descriptions. Pricing/quota changes for new models are typically committed as `feat:` touching `index.html` and the README's feature list.
