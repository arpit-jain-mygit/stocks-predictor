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

| Phase | Parent Outcome | Features (linked) | Stories (linked) | Backlog (linked) |
|---|---|---|---|---|
| Phase 0 (Week 1) | Lock MVP scope, prediction definitions, and stock universe | <a href="#f1">Stock Selection</a>, <a href="#f2">User Profit % Configuration</a>, <a href="#f3">Buy GTT Suggestion</a>, <a href="#f4">Sell GTT Target Prediction</a>, <a href="#f5">Explanation Layer</a> | <a href="#s-a1">Select a stock</a>, <a href="#s-a2">Set desired profit %</a>, <a href="#s-a3">Get suggested Buy GTT</a>, <a href="#s-a4">Predict Sell GTT target timing</a>, <a href="#s-a5">Understand prediction confidence</a> | <a href="#b-p0-product-data">P0 Product and Data Foundations</a> (scope, contract, labels, provider selection) |
| Phase 1 (Weeks 2-4) | Build reliable historical + daily data foundation | <a href="#f6">Historical + Daily Data Pipeline</a>, <a href="#f1">Stock Selection</a> | <a href="#s-b1">Daily market data refresh</a>, <a href="#s-b2">Historical backfill for supported stocks</a>, <a href="#s-a1">Select a stock</a> | <a href="#b-p0-product-data">P0 Product and Data Foundations</a> (schema, mapping, backfill, refresh, data quality) |
| Phase 2 (Weeks 4-7) | Train baseline probability + timing models | <a href="#f3">Buy GTT Suggestion</a>, <a href="#f4">Sell GTT Target Prediction</a>, <a href="#f6">Historical + Daily Data Pipeline</a>, <a href="#f7">Backtesting Dashboard</a> | <a href="#s-a3">Get suggested Buy GTT</a>, <a href="#s-a4">Predict Sell GTT target timing</a>, <a href="#s-b3">Backtesting before release</a> | <a href="#b-p0-modeling">P0 Modeling MVP</a> |
| Phase 3 (Weeks 7-10) | Deliver MVP API + UI for prediction flow | <a href="#f1">Stock Selection</a>, <a href="#f2">User Profit % Configuration</a>, <a href="#f3">Buy GTT Suggestion</a>, <a href="#f4">Sell GTT Target Prediction</a>, <a href="#f5">Explanation Layer</a> | <a href="#s-a1">Select a stock</a>, <a href="#s-a2">Set desired profit %</a>, <a href="#s-a3">Get suggested Buy GTT</a>, <a href="#s-a4">Predict Sell GTT target timing</a>, <a href="#s-a5">Understand prediction confidence</a> | <a href="#b-p0-api-ux">P0 API and UX MVP</a> |
| Phase 4 (Weeks 10-12) | Add trust, backtesting, and observability for launch readiness | <a href="#f7">Backtesting Dashboard</a>, <a href="#f5">Explanation Layer</a> | <a href="#s-b3">Backtesting before release</a>, <a href="#s-a5">Understand prediction confidence</a> | <a href="#b-p1-reliability">P1 Reliability and Observability</a> |
| Phase 5 (Post-MVP) | Expand to personalization, alerts, and advanced modeling | <a href="#f8">Alerts and Notifications</a>, <a href="#f9">User Preferences</a>, <a href="#f10">Watchlist Predictions</a>, <a href="#f11">Stop-loss and Risk/Reward Optimization</a>, <a href="#f12">Personalized Models</a>, <a href="#f13">Event-aware Forecasting</a> | <a href="#s-c1">Save default profit %</a>, <a href="#s-c2">Compare multiple profit % scenarios</a> | <a href="#b-p1-ux-enhancements">P1 User Experience Enhancements</a>, <a href="#b-p2-advanced">P2 Advanced Features</a> |

### Phase Exit Criteria (Simple View)

| Phase | Exit Criteria |
|---|---|
| Phase 0 | Scope, prediction contract, labels, and data source decisions are documented and approved. |
| Phase 1 | Backfill + daily refresh + symbol normalization + data quality checks run reliably for MVP universe. |
| Phase 2 | Baseline models are trained, evaluated, calibrated, and versioned; walk-forward backtest runs end-to-end. |
| Phase 3 | User can request and view Buy GTT + Sell GTT probability/timing with confidence and disclaimer. |
| Phase 4 | Backtesting reporting, observability, and drift checks support launch readiness decisions. |
| Phase 5 | Post-MVP features are shipped in prioritized releases with cost/usage controls. |

## Features

## MVP Features (Must Have)

<a id="f1"></a>
### 1. Stock Selection (Indian Equities)
- Search/select stock by symbol and company name
- Support initial universe (e.g., NIFTY 50 / curated list)
- Display latest close and recent trend summary

<a id="f2"></a>
### 2. User Profit % Configuration
- User enters desired profit percentage per transaction
- Validation for min/max range (example: 1% to 25%)
- Profit % stored per prediction request and optionally as default preference

<a id="f3"></a>
### 3. Buy GTT Suggestion
- Suggest a Buy GTT trigger price based on candidate entry rules + model ranking
- Show confidence and expected fill window
- Show assumptions used (horizon, volatility regime)

<a id="f4"></a>
### 4. Sell GTT Target Prediction
- Compute sell target price from buy price and user profit %
- Predict probability of target hit within horizons (5/10/20 trading days)
- Predict expected time to target hit

<a id="f5"></a>
### 5. Explanation Layer (GenAI-assisted)
- Natural language explanation of prediction drivers
- Plain-language risk notes and confidence interpretation
- Explain how profit % affects probability/timing

<a id="f6"></a>
### 6. Historical + Daily Data Pipeline
- Historical backfill ingestion
- Daily incremental updates
- Data quality checks for missing/duplicate candles

<a id="f7"></a>
### 7. Backtesting Dashboard (Internal MVP)
- Hit-rate by stock and horizon
- Calibration checks (predicted vs actual)
- Performance by market regime/time window

## V1.1 Features (Should Have)

<a id="f8"></a>
### 8. Alerts and Notifications
- Notify users when predicted target window approaches
- Notify when confidence drops materially after market move

<a id="f9"></a>
### 9. User Preferences
- Default profit %
- Preferred horizon
- Risk appetite (conservative/balanced/aggressive)

<a id="f10"></a>
### 10. Watchlist Predictions
- Batch predictions for a user watchlist
- Rank opportunities by probability x expected time

## Future Features (Could Have)

<a id="f11"></a>
### 11. Stop-loss and Risk/Reward Optimization
- User-configurable stop-loss %
- Recommend trade only if risk/reward and probability criteria met

<a id="f12"></a>
### 12. Personalized Models
- Personalization based on user behavior, holding period, and risk profile

<a id="f13"></a>
### 13. Event-aware Forecasting
- Earnings calendar, corporate announcements, macro signals, news sentiment

## User Stories

## Epic A: Core Prediction Experience

<a id="s-a1"></a>
### Story A1: Select a stock
As a user, I want to search and select an Indian stock so that I can request a prediction for that stock.

Acceptance Criteria:
- I can search by symbol or company name.
- The system shows matching supported stocks.
- Selecting a stock loads latest market snapshot (at minimum latest close and date).

<a id="s-a2"></a>
### Story A2: Set desired profit %
As a user, I want to enter my desired profit percentage for a transaction so that the prediction is tailored to my goal.

Acceptance Criteria:
- I can enter a profit % value.
- Invalid inputs are rejected with a clear message.
- The chosen profit % is used in the prediction output.

<a id="s-a3"></a>
### Story A3: Get suggested Buy GTT
As a user, I want a suggested Buy GTT price so that I know a reasonable entry trigger to place.

Acceptance Criteria:
- The system returns a suggested Buy GTT price.
- The response includes confidence and a prediction timestamp.
- The response includes a short explanation of the suggestion basis.

<a id="s-a4"></a>
### Story A4: Predict Sell GTT target trigger timing
As a user, I want to know the probability and expected time for my sell target (based on profit %) to be hit so that I can plan exits.

Acceptance Criteria:
- The system returns computed sell target price.
- The system returns probability of hit within at least one horizon.
- The system returns expected trading days to hit or indicates low confidence/no estimate.

<a id="s-a5"></a>
### Story A5: Understand prediction confidence
As a user, I want the predictor to explain why confidence is high or low so that I can make informed decisions.

Acceptance Criteria:
- The explanation is plain language and understandable.
- It mentions key drivers (trend/volatility/volume context).
- It includes a non-advisory disclaimer.

## Epic B: Data and Reliability

<a id="s-b1"></a>
### Story B1: Daily market data refresh
As a platform operator, I want daily stock data to refresh automatically so that predictions use recent data.

Acceptance Criteria:
- A scheduled job ingests the latest daily candles.
- Missing/duplicate rows are detected and flagged.
- A refresh status is visible in logs/monitoring.

<a id="s-b2"></a>
### Story B2: Historical backfill for supported stocks
As a platform operator, I want historical data backfilled so that the model can train on sufficient history.

Acceptance Criteria:
- Backfill runs for supported stock universe.
- Corporate actions adjustments are handled or clearly documented.
- Data coverage report is generated.

<a id="s-b3"></a>
### Story B3: Backtesting before release
As a platform operator, I want walk-forward backtests so that we can validate the predictor before user launch.

Acceptance Criteria:
- Backtests avoid future leakage.
- Metrics are generated by time period and stock.
- Calibration metrics are included for probability outputs.

## Epic C: User Preferences and UX

<a id="s-c1"></a>
### Story C1: Save default profit %
As a returning user, I want to save a default profit % so that I do not need to re-enter it each time.

Acceptance Criteria:
- User can set and update default profit %.
- New predictions prefill that value.
- Manual override for individual predictions is still allowed.

<a id="s-c2"></a>
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

<a id="b-p0-product-data"></a>
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

<a id="b-p0-modeling"></a>
## P0 - Modeling MVP

- [ ] Generate feature set (trend, momentum, volatility, volume)
- [ ] Include `profit_pct` and forecast horizon as model inputs
- [ ] Train baseline classifier: target hit within horizon (yes/no)
- [ ] Train baseline timing model: expected days to hit target
- [ ] Calibrate classifier probabilities (Platt/isotonic)
- [ ] Build offline evaluation report template
- [ ] Implement walk-forward backtesting pipeline
- [ ] Save model artifacts and version metadata

<a id="b-p0-api-ux"></a>
## P0 - API and UX MVP

- [ ] Define request/response schema for `/predict_gtt`
- [ ] Implement prediction service endpoint (stock + profit % input)
- [ ] Implement buy GTT candidate generation and ranking
- [ ] Compute sell target price and horizon probabilities
- [ ] Add explanation payload fields for LLM summary layer
- [ ] Build MVP UI form (stock, profit %, horizon)
- [ ] Build MVP result card (buy GTT, target, probability, expected days, confidence)
- [ ] Add disclaimer and risk messaging in UI

<a id="b-p1-reliability"></a>
## P1 - Reliability and Observability

- [ ] Add scheduled retraining workflow (weekly/monthly)
- [ ] Add model inference logging and request trace IDs
- [ ] Add monitoring for data freshness and prediction failures
- [ ] Add drift checks (feature distribution and calibration drift)
- [ ] Add admin/internal metrics dashboard

<a id="b-p1-ux-enhancements"></a>
## P1 - User Experience Enhancements

- [ ] Save user default preferences (profit %, horizon, risk profile)
- [ ] Multi-scenario compare view (2-3 profit % targets)
- [ ] Watchlist batch prediction endpoint
- [ ] Notification service for predicted target windows

<a id="b-p2-advanced"></a>
## P2 - Advanced Features

- [ ] Stop-loss-aware probability and risk/reward modeling
- [ ] Sector/index regime features and macro overlays
- [ ] Event-aware features (earnings/news/sentiment)
- [ ] Personalized recommendations based on user behavior
- [ ] Portfolio-level ranking and allocation suggestions
