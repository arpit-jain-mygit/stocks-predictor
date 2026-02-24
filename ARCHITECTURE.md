# Architecture

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

```mermaid
flowchart LR
  %% Personas
  P1["Persona 1: Retail Trader\nGoal: Buy GTT + profit% sell timing\nUses: F-01 F-02 F-03 F-04 F-05\nStories: S-A1 S-A2 S-A3 S-A4 S-A5"]
  P2["Persona 2: Power User / Active Trader\nGoal: compare scenarios, watchlist insights\nUses: F-08 F-09 F-10 F-11 F-12 F-13\nStories: S-C1 S-C2"]
  P3["Persona 3: Product/Admin Operator\nGoal: freshness, reliability, backtesting, monitoring\nUses: F-06 F-07 F-05\nStories: S-B1 S-B2 S-B3"]

  %% Runtime platform components
  UI["Web UI (Vercel, web/)\nSystem role: input + result rendering\nPhases: PH-3 PH-5\nFeatures: F-01 F-02 F-03 F-04 F-05 F-08 F-09 F-10\nStories: S-A1 S-A2 S-A3 S-A4 S-A5 S-C1 S-C2\nBacklog: STK-023 STK-024 STK-025 STK-030 STK-031 STK-032"]
  API["API Service (Render, api/)\nSystem role: validate, orchestrate inference, assemble response\nPhases: PH-3 PH-4 PH-5\nFeatures: F-03 F-04 F-05 F-08 F-10\nStories: S-A2 S-A3 S-A4 S-A5 S-C2\nBacklog: STK-018 STK-019 STK-020 STK-021 STK-022 STK-027 STK-028 STK-033 STK-034"]
  CACHE["Redis Cache (Upstash)\nSystem role: cache predictions/explanations, rate-limit\nPhases: PH-3 PH-4\nFeatures: F-05 F-07\nStories: S-A5 S-B3\nBacklog: STK-022 STK-027 STK-028"]
  VERTEX["Vertex AI (GCP)\nSystem role: explanation/Q&A layer (not core predictor)\nPhases: PH-3 PH-4\nFeatures: F-05\nStories: S-A5\nBacklog: STK-022 STK-025 STK-027"]
  DB["Postgres (Neon/Supabase)\nSystem role: users, preferences, prediction metadata\nPhases: PH-3 PH-5\nFeatures: F-09 F-10\nStories: S-C1 S-C2\nBacklog: STK-031 STK-032 STK-033"]
  OBJ["Object Storage (R2 / Supabase Storage)\nSystem role: model artifacts, snapshots, reports\nPhases: PH-2 PH-4\nFeatures: F-04 F-07\nStories: S-A4 S-B3\nBacklog: STK-017 STK-026 STK-030"]

  %% ML/data/pipelines components
  INFER["ML Inference Layer (ml/inference)\nSystem role: candidate scoring + probability/timing inference\nPhases: PH-2 PH-3\nFeatures: F-03 F-04\nStories: S-A3 S-A4\nBacklog: STK-011 STK-012 STK-013 STK-019 STK-020 STK-021"]
  FEATURES["Feature Store / Feature Tables (data+ml/features)\nSystem role: latest inference features + training features\nPhases: PH-1 PH-2\nFeatures: F-06\nStories: S-B1 S-B2 S-B3\nBacklog: STK-005 STK-006 STK-007 STK-008 STK-009 STK-010"]
  TRAIN["Training + Calibration + Backtest (ml/training)\nSystem role: train, calibrate, validate, version\nPhases: PH-2 PH-4\nFeatures: F-04 F-07\nStories: S-A4 S-A5 S-B3\nBacklog: STK-012 STK-013 STK-014 STK-015 STK-016 STK-017 STK-026 STK-029"]
  DATAING["Data Ingestion + Normalization (data/, pipelines/)\nSystem role: backfill + daily refresh + DQ checks\nPhases: PH-1\nFeatures: F-06\nStories: S-B1 S-B2\nBacklog: STK-004 STK-005 STK-006 STK-007 STK-008 STK-009"]
  SCHED["Schedulers / CI Jobs (GitHub Actions / Cron)\nSystem role: daily refresh, weekly retrain, monitoring jobs\nPhases: PH-1 PH-2 PH-4\nFeatures: F-06 F-07\nStories: S-B1 S-B2 S-B3\nBacklog: STK-008 STK-016 STK-026 STK-028 STK-029"]
  MON["Monitoring + Admin Metrics (infra + dashboards)\nSystem role: freshness, inference failures, drift, ops visibility\nPhases: PH-4\nFeatures: F-07\nStories: S-B3\nBacklog: STK-027 STK-028 STK-029 STK-030"]
  MARKET["Market Data Providers (OHLCV + corporate actions)\nSystem role: upstream market data source\nPhases: PH-0 PH-1\nFeatures: F-06\nStories: S-B1 S-B2\nBacklog: STK-004"]

  %% Runtime user journey
  P1 --> UI
  P2 --> UI
  P3 --> MON
  UI -->|"symbol + profit_pct + horizon\n(Runtime request)"| API
  API --> CACHE
  API --> DB
  API --> FEATURES
  API --> INFER
  INFER --> FEATURES
  INFER --> OBJ
  API -->|"compact prediction summary\n(explanation request)"| VERTEX
  VERTEX --> API
  API -->|"prediction payload + explanation + disclaimer"| UI

  %% Offline data/ML journey
  MARKET --> DATAING
  DATAING --> FEATURES
  SCHED --> DATAING
  SCHED --> TRAIN
  FEATURES --> TRAIN
  TRAIN --> OBJ
  TRAIN --> DB
  TRAIN --> MON
  SCHED --> MON
  API --> MON

  %% Optional future flows
  P2 -. "watchlist/scenario compare" .-> API
  API -. "alert windows" .-> DB

  %% Visual grouping: node colors
  classDef persona fill:#FFF3BF,stroke:#B08900,stroke-width:1.5px,color:#3D2C00;
  classDef product fill:#DCEEFF,stroke:#1D4ED8,stroke-width:1.5px,color:#0B1F44;
  classDef dataflow fill:#DCFCE7,stroke:#15803D,stroke-width:1.5px,color:#052E16;
  classDef external fill:#FCE7F3,stroke:#BE185D,stroke-width:1.5px,color:#4A044E;
  classDef ops fill:#F3E8FF,stroke:#7E22CE,stroke-width:1.5px,color:#3B0764;

  class P1,P2,P3 persona;
  class UI,API product;
  class INFER,FEATURES,TRAIN,DATAING dataflow;
  class SCHED,MON ops;
  class CACHE,VERTEX,DB,OBJ,MARKET external;

  %% Visual grouping: link colors
  %% Persona interaction links (0-2)
  linkStyle 0,1,2 stroke:#B08900,stroke-width:2px;
  %% Runtime request/response and inference flow (3-13)
  linkStyle 3,4,5,6,7,8,9,10,11,12,13 stroke:#1D4ED8,stroke-width:2px;
  %% Offline data + training + monitoring flow (14-23)
  linkStyle 14,15,16,17,18,19,20,21,22,23 stroke:#15803D,stroke-width:2px;
  %% Future optional flows (24-25)
  linkStyle 24,25 stroke:#7E22CE,stroke-width:2px,stroke-dasharray: 5 5;
```

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
