<img width="1536" height="1024" alt="ChatGPT Image Feb 24, 2026, 08_46_30 AM" src="https://github.com/user-attachments/assets/6946ecba-5402-4827-a44d-3fc0ab1579bc" /># Architecture

## Goal

Build a probabilistic stock prediction platform for Indian equities that recommends:
- a Buy GTT trigger
- the likelihood and expected timing for a user-configured profit % Sell GTT target

## Product Scope (MVP)

Inputs:
- stock symbol (Indian equity, e.g. NSE symbols)
- user desired `profit_pct`
- prediction horizon (e.g. 5/10/20 trading days)

Outputs:
- suggested Buy GTT
- sell target price (derived from buy GTT and `profit_pct`)
- probability target is hit within horizon(s)
- expected days to hit target
- confidence + explanation

## Unified System Integration + Data Flow Journey (with Personas)

<img width="1536" height="1024" alt="ChatGPT Image Feb 24, 2026, 09_26_22 AM" src="https://github.com/user-attachments/assets/ff9b1751-9081-42a1-9249-0114d8442206" />


## Diagram Notes (How to read it)

- Runtime user journey is left-to-right across `UI -> API -> Inference/Vertex -> UI`.
- Offline data and model lifecycle is lower flow: `Market Data -> Data Ingestion -> Features -> Train/Backtest -> Artifacts/Monitoring`.
- Colors:
  - Yellow = User personas
  - Blue = Product/feature-delivery components (`web`, `api`)
  - Green = Data/ML flow components
  - Pink = External systems / managed services / upstream providers
  - Purple = Ops/monitoring components and future-flow edges
- Each component node is annotated with:
  - `Phases` (`PH-*`)
  - `Features` (`F-*`)
  - `Stories` (`S-*`)
  - `Backlog` items (`STK-*`)
- This makes it possible to trace any story/feature to the exact system component(s) and platform deployment.

## High-Level Components

### 1. Data Layer (`data/`)
- Historical OHLCV backfill
- Daily incremental refresh
- Symbol normalization and exchange mapping
- Feature-ready cleaned candles

### 2. ML Layer (`ml/`)
- Feature generation (trend, momentum, volatility, volume)
- Label generation for target-hit and time-to-target
- Baseline models:
  - classifier for target hit probability
  - regressor/survival model for expected days to hit
- Calibration and backtesting

### 3. API Layer (`api/`)
- Prediction endpoint (`/predict_gtt`)
- Health/version endpoints
- Response schema with confidence and explanation fields

### 4. Web/UI Layer (`web/`)
- Stock selector
- Profit % and horizon inputs
- Prediction result cards
- Explanation and risk disclosure

### 5. Ops Layer (`infra/`, `scripts/`)
- Scheduled data refresh and retraining jobs
- Logging and monitoring hooks
- Deployment configuration

## Prediction Flow (MVP)

1. User selects a stock and profit %.
2. System generates candidate Buy GTT entries (rule-based candidates).
3. ML model scores each candidate for:
   - fill probability
   - target-hit probability (using `profit_pct`)
   - expected time-to-target
4. Best candidate is selected based on objective (probability, time, risk policy).
5. API returns prediction payload.
6. GenAI explanation layer summarizes the result in plain language.

## Data and Modeling Notes

- `profit_pct` must be a model input (not just post-processing).
- Use walk-forward validation to avoid leakage.
- Treat outputs as probabilities/confidence, not guarantees.
- India market specifics (trading holidays, symbol conventions, splits/bonuses) must be normalized in the data layer.

## Suggested Initial Stack

- Python (`pandas`, `numpy`, `scikit-learn`, `xgboost/lightgbm`)
- FastAPI for prediction API
- Postgres/Timescale (or parquet in MVP experiments)
- React for frontend (later iteration)
- Scheduler for daily refresh and retraining (cron/Prefect/Celery)

## Proposed Repo Layout

- `api/` API application and schemas
- `ml/` model training/inference/features
- `data/` local data assets and pipelines
- `web/` frontend app (future)
- `docs/` specs and experiments
- `tests/` unit/integration tests
- `scripts/` repeatable ops commands
