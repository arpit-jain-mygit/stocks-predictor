# Product Plan

This document consolidates the planning artifacts for the Stocks Predictor MVP and near-term roadmap.

## Table of Contents

- [Roadmap](#roadmap)
- [Phase Delivery Traceability](#phase-delivery-traceability)
- [Features](#features)
- [User Stories](#user-stories)
- [Backlog](#backlog)

## Roadmap

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

## Phase Delivery Traceability

This section links each roadmap phase to the features, stories, and backlog items required to deliver it.

### Phase 0 -> Product Definition and Research (Week 1)

Parent outcome:
- lock MVP scope, prediction definitions, and stock universe

Features in scope:
- Feature 1: Stock Selection (scope definition only: supported universe)
- Feature 2: User Profit % Configuration (input constraints and UX rules)
- Feature 3: Buy GTT Suggestion (definition of what is being suggested)
- Feature 4: Sell GTT Target Prediction (definition of probability + timing outputs)
- Feature 5: Explanation Layer (scope boundary for GenAI usage)

Stories driving this phase:
- Story A1: Select a stock (define supported search/symbol behavior)
- Story A2: Set desired profit % (define validation and persistence expectations)
- Story A3: Get suggested Buy GTT (define response fields and explanation expectations)
- Story A4: Predict Sell GTT target trigger timing (define probability/timing response expectations)
- Story A5: Understand prediction confidence (define explanation/disclaimer expectations)

Backlog items to complete in this phase:
- Define MVP stock universe (e.g., NIFTY 50 vs curated top liquid stocks)
- Finalize prediction contract for `predict_gtt` response
- Define label generation logic for target-hit and days-to-hit
- Select and document data providers for Indian daily OHLCV + corporate actions

Phase 0 deliverable checklist:
- PRD-level prediction contract drafted
- stock universe list approved
- label definitions documented
- data source decision documented

### Phase 1 -> Data Foundation (Weeks 2-4)

Parent outcome:
- reliable historical + daily data pipeline for supported Indian stocks

Features in scope:
- Feature 6: Historical + Daily Data Pipeline
- Feature 1: Stock Selection (data readiness for symbol lookup and latest snapshot)

Stories delivered/enabled:
- Story B1: Daily market data refresh
- Story B2: Historical backfill for supported stocks
- Story A1: Select a stock (latest market snapshot dependency)

Backlog items to complete in this phase:
- Implement historical data schema (raw + cleaned candles)
- Implement symbol normalization and exchange mapping (NSE/BSE)
- Build historical backfill job
- Build daily incremental refresh job
- Add data quality checks (missing dates, duplicates, outliers)

Phase 1 deliverable checklist:
- reproducible backfill works for MVP universe
- daily refresh pipeline runs on schedule
- data quality checks flag bad/missing rows
- symbol mapping supports chosen exchange symbols

### Phase 2 -> MVP Modeling (Weeks 4-7)

Parent outcome:
- baseline prediction models for target-hit probability and time-to-target

Features in scope:
- Feature 3: Buy GTT Suggestion (candidate scoring foundation)
- Feature 4: Sell GTT Target Prediction
- Feature 6: Historical + Daily Data Pipeline (feature generation dependency)
- Feature 7: Backtesting Dashboard (internal metrics inputs begin here)

Stories delivered/enabled:
- Story A3: Get suggested Buy GTT (baseline scoring logic)
- Story A4: Predict Sell GTT target trigger timing
- Story B3: Backtesting before release (pipeline and metrics foundation)

Backlog items to complete in this phase:
- Generate feature set (trend, momentum, volatility, volume)
- Include `profit_pct` and forecast horizon as model inputs
- Train baseline classifier: target hit within horizon (yes/no)
- Train baseline timing model: expected days to hit target
- Calibrate classifier probabilities (Platt/isotonic)
- Build offline evaluation report template
- Implement walk-forward backtesting pipeline
- Save model artifacts and version metadata

Phase 2 deliverable checklist:
- baseline models trained and versioned
- offline metrics report generated
- walk-forward backtest runs end-to-end
- calibrated probability output available for inference

### Phase 3 -> Prediction API + UX MVP (Weeks 7-10)

Parent outcome:
- user-facing MVP that returns buy GTT + sell target probability/timing

Features in scope:
- Feature 1: Stock Selection (UI + API integration)
- Feature 2: User Profit % Configuration
- Feature 3: Buy GTT Suggestion
- Feature 4: Sell GTT Target Prediction
- Feature 5: Explanation Layer (initial GenAI-assisted explanation)

Stories delivered:
- Story A1: Select a stock
- Story A2: Set desired profit %
- Story A3: Get suggested Buy GTT
- Story A4: Predict Sell GTT target trigger timing
- Story A5: Understand prediction confidence

Backlog items to complete in this phase:
- Define request/response schema for `/predict_gtt`
- Implement prediction service endpoint (stock + profit % input)
- Implement buy GTT candidate generation and ranking
- Compute sell target price and horizon probabilities
- Add explanation payload fields for LLM summary layer
- Build MVP UI form (stock, profit %, horizon)
- Build MVP result card (buy GTT, target, probability, expected days, confidence)
- Add disclaimer and risk messaging in UI

Phase 3 deliverable checklist:
- API returns prediction payload for MVP universe symbols
- UI can request and render predictions
- confidence + disclaimer shown in results
- explanation payload available (generated or placeholder)

### Phase 4 -> Backtesting and Trust Features (Weeks 10-12)

Parent outcome:
- trust and validation layer for launch readiness

Features in scope:
- Feature 7: Backtesting Dashboard (Internal MVP)
- Feature 5: Explanation Layer (improved confidence messaging)

Stories delivered/enabled:
- Story B3: Backtesting before release (full release gate)
- Story A5: Understand prediction confidence (improved calibration messaging)

Backlog items to complete in this phase:
- Add scheduled retraining workflow (weekly/monthly)
- Add model inference logging and request trace IDs
- Add monitoring for data freshness and prediction failures
- Add drift checks (feature distribution and calibration drift)
- Add admin/internal metrics dashboard

Phase 4 deliverable checklist:
- backtesting reports generated on schedule
- basic observability and drift checks in place
- internal dashboard available for model quality review
- trust messaging updated using calibration outputs

### Phase 5 -> Expansion (Post-MVP)

Parent outcome:
- richer personalization, alerts, and risk-aware product capabilities

Features in scope:
- Feature 8: Alerts and Notifications
- Feature 9: User Preferences
- Feature 10: Watchlist Predictions
- Feature 11: Stop-loss and Risk/Reward Optimization
- Feature 12: Personalized Models
- Feature 13: Event-aware Forecasting

Stories delivered/enabled:
- Story C1: Save default profit %
- Story C2: Compare multiple profit % scenarios
- Additional stories to be added for alerts/watchlists/personalization

Backlog items to complete in this phase:
- Save user default preferences (profit %, horizon, risk profile)
- Multi-scenario compare view (2-3 profit % targets)
- Watchlist batch prediction endpoint
- Notification service for predicted target windows
- Stop-loss-aware probability and risk/reward modeling
- Sector/index regime features and macro overlays
- Event-aware features (earnings/news/sentiment)
- Personalized recommendations based on user behavior
- Portfolio-level ranking and allocation suggestions

Phase 5 deliverable checklist:
- post-MVP features prioritized into releases
- personalization and alerts scoped with usage/cost guardrails
- advanced modeling work gated by measured MVP adoption

## Features

## MVP Features (Must Have)

### 1. Stock Selection (Indian Equities)
- Search/select stock by symbol and company name
- Support initial universe (e.g., NIFTY 50 / curated list)
- Display latest close and recent trend summary

### 2. User Profit % Configuration
- User enters desired profit percentage per transaction
- Validation for min/max range (example: 1% to 25%)
- Profit % stored per prediction request and optionally as default preference

### 3. Buy GTT Suggestion
- Suggest a Buy GTT trigger price based on candidate entry rules + model ranking
- Show confidence and expected fill window
- Show assumptions used (horizon, volatility regime)

### 4. Sell GTT Target Prediction
- Compute sell target price from buy price and user profit %
- Predict probability of target hit within horizons (5/10/20 trading days)
- Predict expected time to target hit

### 5. Explanation Layer (GenAI-assisted)
- Natural language explanation of prediction drivers
- Plain-language risk notes and confidence interpretation
- Explain how profit % affects probability/timing

### 6. Historical + Daily Data Pipeline
- Historical backfill ingestion
- Daily incremental updates
- Data quality checks for missing/duplicate candles

### 7. Backtesting Dashboard (Internal MVP)
- Hit-rate by stock and horizon
- Calibration checks (predicted vs actual)
- Performance by market regime/time window

## V1.1 Features (Should Have)

### 8. Alerts and Notifications
- Notify users when predicted target window approaches
- Notify when confidence drops materially after market move

### 9. User Preferences
- Default profit %
- Preferred horizon
- Risk appetite (conservative/balanced/aggressive)

### 10. Watchlist Predictions
- Batch predictions for a user watchlist
- Rank opportunities by probability x expected time

## Future Features (Could Have)

### 11. Stop-loss and Risk/Reward Optimization
- User-configurable stop-loss %
- Recommend trade only if risk/reward and probability criteria met

### 12. Personalized Models
- Personalization based on user behavior, holding period, and risk profile

### 13. Event-aware Forecasting
- Earnings calendar, corporate announcements, macro signals, news sentiment

## User Stories

## Epic A: Core Prediction Experience

### Story A1: Select a stock
As a user, I want to search and select an Indian stock so that I can request a prediction for that stock.

Acceptance Criteria:
- I can search by symbol or company name.
- The system shows matching supported stocks.
- Selecting a stock loads latest market snapshot (at minimum latest close and date).

### Story A2: Set desired profit %
As a user, I want to enter my desired profit percentage for a transaction so that the prediction is tailored to my goal.

Acceptance Criteria:
- I can enter a profit % value.
- Invalid inputs are rejected with a clear message.
- The chosen profit % is used in the prediction output.

### Story A3: Get suggested Buy GTT
As a user, I want a suggested Buy GTT price so that I know a reasonable entry trigger to place.

Acceptance Criteria:
- The system returns a suggested Buy GTT price.
- The response includes confidence and a prediction timestamp.
- The response includes a short explanation of the suggestion basis.

### Story A4: Predict Sell GTT target trigger timing
As a user, I want to know the probability and expected time for my sell target (based on profit %) to be hit so that I can plan exits.

Acceptance Criteria:
- The system returns computed sell target price.
- The system returns probability of hit within at least one horizon.
- The system returns expected trading days to hit or indicates low confidence/no estimate.

### Story A5: Understand prediction confidence
As a user, I want the predictor to explain why confidence is high or low so that I can make informed decisions.

Acceptance Criteria:
- The explanation is plain language and understandable.
- It mentions key drivers (trend/volatility/volume context).
- It includes a non-advisory disclaimer.

## Epic B: Data and Reliability

### Story B1: Daily market data refresh
As a platform operator, I want daily stock data to refresh automatically so that predictions use recent data.

Acceptance Criteria:
- A scheduled job ingests the latest daily candles.
- Missing/duplicate rows are detected and flagged.
- A refresh status is visible in logs/monitoring.

### Story B2: Historical backfill for supported stocks
As a platform operator, I want historical data backfilled so that the model can train on sufficient history.

Acceptance Criteria:
- Backfill runs for supported stock universe.
- Corporate actions adjustments are handled or clearly documented.
- Data coverage report is generated.

### Story B3: Backtesting before release
As a platform operator, I want walk-forward backtests so that we can validate the predictor before user launch.

Acceptance Criteria:
- Backtests avoid future leakage.
- Metrics are generated by time period and stock.
- Calibration metrics are included for probability outputs.

## Epic C: User Preferences and UX

### Story C1: Save default profit %
As a returning user, I want to save a default profit % so that I do not need to re-enter it each time.

Acceptance Criteria:
- User can set and update default profit %.
- New predictions prefill that value.
- Manual override for individual predictions is still allowed.

### Story C2: Compare multiple profit % scenarios
As a user, I want to compare outcomes for different profit % values so that I can choose a realistic target.

Acceptance Criteria:
- I can request at least 2-3 scenarios (e.g., 4%, 8%, 12%).
- The UI shows probability/time comparisons side-by-side.
- The explanation highlights the tradeoff between higher profit and lower hit probability.

## Backlog

## Prioritization Scheme
- `P0` = critical for MVP path
- `P1` = important, near-term
- `P2` = useful enhancement

## P0 - Product and Data Foundations

- [ ] Define MVP stock universe (e.g., NIFTY 50 vs curated top liquid stocks)
- [ ] Finalize prediction contract for `predict_gtt` response
- [ ] Define label generation logic for target-hit and days-to-hit
- [ ] Select and document data providers for Indian daily OHLCV + corporate actions
- [ ] Implement historical data schema (raw + cleaned candles)
- [ ] Implement symbol normalization and exchange mapping (NSE/BSE)
- [ ] Build historical backfill job
- [ ] Build daily incremental refresh job
- [ ] Add data quality checks (missing dates, duplicates, outliers)

## P0 - Modeling MVP

- [ ] Generate feature set (trend, momentum, volatility, volume)
- [ ] Include `profit_pct` and forecast horizon as model inputs
- [ ] Train baseline classifier: target hit within horizon (yes/no)
- [ ] Train baseline timing model: expected days to hit target
- [ ] Calibrate classifier probabilities (Platt/isotonic)
- [ ] Build offline evaluation report template
- [ ] Implement walk-forward backtesting pipeline
- [ ] Save model artifacts and version metadata

## P0 - API and UX MVP

- [ ] Define request/response schema for `/predict_gtt`
- [ ] Implement prediction service endpoint (stock + profit % input)
- [ ] Implement buy GTT candidate generation and ranking
- [ ] Compute sell target price and horizon probabilities
- [ ] Add explanation payload fields for LLM summary layer
- [ ] Build MVP UI form (stock, profit %, horizon)
- [ ] Build MVP result card (buy GTT, target, probability, expected days, confidence)
- [ ] Add disclaimer and risk messaging in UI

## P1 - Reliability and Observability

- [ ] Add scheduled retraining workflow (weekly/monthly)
- [ ] Add model inference logging and request trace IDs
- [ ] Add monitoring for data freshness and prediction failures
- [ ] Add drift checks (feature distribution and calibration drift)
- [ ] Add admin/internal metrics dashboard

## P1 - User Experience Enhancements

- [ ] Save user default preferences (profit %, horizon, risk profile)
- [ ] Multi-scenario compare view (2-3 profit % targets)
- [ ] Watchlist batch prediction endpoint
- [ ] Notification service for predicted target windows

## P2 - Advanced Features

- [ ] Stop-loss-aware probability and risk/reward modeling
- [ ] Sector/index regime features and macro overlays
- [ ] Event-aware features (earnings/news/sentiment)
- [ ] Personalized recommendations based on user behavior
- [ ] Portfolio-level ranking and allocation suggestions
