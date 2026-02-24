# Stocks Predictor

Monorepo for a GenAI-assisted Indian stock decision-support product that predicts:
- suggested `Buy GTT`
- user-configurable profit% `Sell GTT` target outcomes
- probability and expected time-to-target

This is a probabilistic prediction system (not guaranteed financial advice).

## Table of Contents

- [Product Vision](#product-vision)
- [Monorepo Strategy](#monorepo-strategy)
- [SDLC / Agile Working Structure](#sdlc--agile-working-structure)
- [Primary Docs (Read in Order)](#primary-docs-read-in-order)
- [Sprint Execution Workflow](#sprint-execution-workflow)
- [Repo Layout](#repo-layout)
- [Guiding Principles](#guiding-principles)

## Product Vision

Build a user-facing decision support system for Indian equities that combines:
- quantitative forecasting models (probability + timing)
- explainable GenAI summaries (Vertex AI)
- user-configurable profit targets and trading horizons

Core MVP outcome for a selected stock and `profit_pct`:
- suggested Buy GTT price
- sell target price
- probability target is hit within selected horizon(s)
- expected trading days to target
- confidence + explanation + disclaimer

## Monorepo Strategy

This repo keeps all components in one place, while deploying each component separately:
- `web/` -> Vercel
- `api/` -> Render
- `pipelines` / data + training jobs -> scheduled jobs (GitHub Actions / cron / GCP as needed)

Why:
- shared contracts and traceability across UI/API/ML
- faster MVP iteration
- simpler product and engineering planning

## SDLC / Agile Working Structure

Use the docs in this repo by phase:
- `Phase 0` Discovery/Definition: scope, contracts, labels, provider decisions
- `Phase 1` Data Foundation: ingestion, normalization, feature-ready datasets
- `Phase 2` Modeling: baseline models, calibration, backtesting
- `Phase 3` API/UI MVP: prediction flow and user experience
- `Phase 4` Readiness: observability, drift, trust, admin metrics
- `Phase 5` Expansion: personalization, alerts, advanced forecasting

## Primary Docs (Read in Order)

1. [`PRODUCT_PLAN.md`](./PRODUCT_PLAN.md)
   - Single source for roadmap, phase traceability, features, detailed stories, and execution backlog table (`STK-*`)
2. [`ARCHITECTURE.md`](./ARCHITECTURE.md)
   - Unified personas + system integration + runtime/offline data flow diagram (with `PH-*`, `F-*`, `S-*`, `STK-*` mapping)
3. [`docs/MVP_SPEC.md`](./docs/MVP_SPEC.md)
   - MVP API/response contract draft and output field definitions
4. [`DEPLOYMENT_FREE_TIER.md`](./DEPLOYMENT_FREE_TIER.md)
   - Deployment strategy for Vercel + Render + free-tier infra components
5. [`VERTEX_INTEGRATION.md`](./VERTEX_INTEGRATION.md)
   - Vertex AI explanation-layer integration, cost controls, and fallback behavior

## Sprint Execution Workflow

Sprint planning:
- Use `PRODUCT_PLAN.md` -> `Phase Delivery Traceability` + `Backlog Master Table`

Story grooming:
- Use `PRODUCT_PLAN.md` -> `Features` + `User Stories`

Architecture alignment:
- Update `ARCHITECTURE.md` whenever stories/features change

Delivery readiness:
- Update backlog `Status`, `Test`, and `Change Summary` columns in `PRODUCT_PLAN.md`

## Repo Layout

- `web/` - frontend app (Vercel)
- `api/` - prediction API (Render)
- `ml/` - training, inference, calibration, backtesting
- `data/` - ingestion, normalization, storage conventions
- `infra/` - deployment/runtime infrastructure assets
- `scripts/` - repeatable local/dev/ops scripts
- `docs/` - supporting specifications and notes
- `tests/` - test suites by component

## Guiding Principles

- Probabilistic outputs over deterministic price claims
- No data leakage in training/backtesting
- `profit_pct` is a first-class model input
- Explainability and risk disclosure are product requirements
- India-market specifics (NSE/BSE symbols, holidays, corporate actions) must be handled explicitly
