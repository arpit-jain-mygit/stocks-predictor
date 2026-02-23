# Roadmap

## Phase 0: Product Definition and Research (Week 1)

Goals:
- define prediction targets and evaluation metrics
- confirm MVP scope and user journeys
- finalize stock universe for v1 (e.g., NIFTY 50)

Deliverables:
- product requirements draft
- modeling assumptions and label definitions
- data source shortlist (historical + daily refresh)

## Phase 1: Data Foundation (Weeks 2-4)

Goals:
- ingest historical OHLCV data for Indian stocks
- build daily refresh pipeline
- normalize symbols, holidays, and corporate actions

Deliverables:
- raw and cleaned market data tables
- feature generation pipeline (technical + volatility + volume)
- reproducible backfill and daily update jobs

## Phase 2: MVP Modeling (Weeks 4-7)

Goals:
- generate labels for target-hit and time-to-target
- train baseline models for probability and timing prediction
- support configurable `profit_pct` as input

Deliverables:
- baseline classifier and regressor/survival model
- offline evaluation and calibration report
- model artifacts and inference contract

## Phase 3: Prediction API + UX MVP (Weeks 7-10)

Goals:
- expose prediction endpoint for buy GTT and sell target timing
- build simple UI for stock selection and profit % configuration
- show confidence and explanation text

Deliverables:
- API endpoints (`/predict_gtt`, `/explain_prediction`)
- MVP dashboard/UI
- audit logging for predictions

## Phase 4: Backtesting and Trust Features (Weeks 10-12)

Goals:
- walk-forward backtests across market regimes
- improve calibration and risk messaging
- publish model performance summaries in product UI/admin

Deliverables:
- backtesting engine and reports
- calibration improvements
- user-facing disclaimer and confidence presentation

## Phase 5: Expansion (Post-MVP)

Potential expansions:
- portfolio-level recommendations
- stop-loss scenarios and risk/reward optimization
- alerts/notifications for predicted target windows
- sector/index context and event-aware features
- premium tiers and personalized models
