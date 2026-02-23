# Product Plan

This document consolidates the planning artifacts for the Stocks Predictor MVP and near-term roadmap.

## Table of Contents

- [Roadmap](#roadmap)
- [Phase Delivery Traceability](#phase-delivery-traceability)
- [Features](#features)
- [User Stories](#user-stories)
- [Backlog](#backlog)

## Roadmap

<a id="ph-0"></a>
## Phase 0: Product Definition and Research (Week 1)

Goals:
- define prediction targets and evaluation metrics
- confirm MVP scope and user journeys
- finalize stock universe for v1 (e.g., NIFTY 50)

Deliverables:
- product requirements draft
- modeling assumptions and label definitions
- data source shortlist (historical + daily refresh)

<a id="ph-1"></a>
## Phase 1: Data Foundation (Weeks 2-4)

Goals:
- ingest historical OHLCV data for Indian stocks
- build daily refresh pipeline
- normalize symbols, holidays, and corporate actions

Deliverables:
- raw and cleaned market data tables
- feature generation pipeline (technical + volatility + volume)
- reproducible backfill and daily update jobs

<a id="ph-2"></a>
## Phase 2: MVP Modeling (Weeks 4-7)

Goals:
- generate labels for target-hit and time-to-target
- train baseline models for probability and timing prediction
- support configurable `profit_pct` as input

Deliverables:
- baseline classifier and regressor/survival model
- offline evaluation and calibration report
- model artifacts and inference contract

<a id="ph-3"></a>
## Phase 3: Prediction API + UX MVP (Weeks 7-10)

Goals:
- expose prediction endpoint for buy GTT and sell target timing
- build simple UI for stock selection and profit % configuration
- show confidence and explanation text

Deliverables:
- API endpoints (`/predict_gtt`, `/explain_prediction`)
- MVP dashboard/UI
- audit logging for predictions

<a id="ph-4"></a>
## Phase 4: Backtesting and Trust Features (Weeks 10-12)

Goals:
- walk-forward backtests across market regimes
- improve calibration and risk messaging
- publish model performance summaries in product UI/admin

Deliverables:
- backtesting engine and reports
- calibration improvements
- user-facing disclaimer and confidence presentation

<a id="ph-5"></a>
## Phase 5: Expansion (Post-MVP)

Potential expansions:
- portfolio-level recommendations
- stop-loss scenarios and risk/reward optimization
- alerts/notifications for predicted target windows
- sector/index context and event-aware features
- premium tiers and personalized models

## Phase Delivery Traceability

This section links each roadmap phase to the features, stories, and backlog items required to deliver it.

### Traceability Relationship Model

- `Phase -> Feature` is generally many-to-many (a feature can span multiple phases).
- `Feature -> Story` is generally one-to-many (one feature is delivered through multiple stories).
- `Story -> Feature` is often many-to-one, but some stories can support multiple features.
- Some rows currently appear `1:1` only because the initial story set is still small; this table should expand as more stories are added.
- ID convention used in this document: `PH-*` (phase), `F-*` (feature), `S-*` (story).

| Phase (ID) | Parent Outcome | Feature (ID) | Stories (ID + linked) |
|---|---|---|---|
| <a href="#ph-0">PH-0</a> | Lock MVP scope, prediction definitions, and stock universe | <a href="#f1">F-01 Stock Selection</a> | <a href="#s-a1">S-A1 Select a stock</a> |
| <a href="#ph-0">PH-0</a> | Lock MVP scope, prediction definitions, and stock universe | <a href="#f2">F-02 User Profit % Configuration</a> | <a href="#s-a2">S-A2 Set desired profit %</a> |
| <a href="#ph-0">PH-0</a> | Lock MVP scope, prediction definitions, and stock universe | <a href="#f3">F-03 Buy GTT Suggestion</a> | <a href="#s-a3">S-A3 Get suggested Buy GTT</a> |
| <a href="#ph-0">PH-0</a> | Lock MVP scope, prediction definitions, and stock universe | <a href="#f4">F-04 Sell GTT Target Prediction</a> | <a href="#s-a4">S-A4 Predict Sell GTT target timing</a> |
| <a href="#ph-0">PH-0</a> | Lock MVP scope, prediction definitions, and stock universe | <a href="#f5">F-05 Explanation Layer</a> | <a href="#s-a5">S-A5 Understand prediction confidence</a> |
| <a href="#ph-1">PH-1</a> | Build reliable historical + daily data foundation | <a href="#f6">F-06 Historical + Daily Data Pipeline</a> | <a href="#s-b1">S-B1 Daily market data refresh</a>, <a href="#s-b2">S-B2 Historical backfill for supported stocks</a> |
| <a href="#ph-1">PH-1</a> | Build reliable historical + daily data foundation | <a href="#f1">F-01 Stock Selection</a> | <a href="#s-a1">S-A1 Select a stock</a> |
| <a href="#ph-2">PH-2</a> | Train baseline probability + timing models | <a href="#f3">F-03 Buy GTT Suggestion</a> | <a href="#s-a3">S-A3 Get suggested Buy GTT</a> |
| <a href="#ph-2">PH-2</a> | Train baseline probability + timing models | <a href="#f4">F-04 Sell GTT Target Prediction</a> | <a href="#s-a4">S-A4 Predict Sell GTT target timing</a> |
| <a href="#ph-2">PH-2</a> | Train baseline probability + timing models | <a href="#f6">F-06 Historical + Daily Data Pipeline</a> | <a href="#s-b3">S-B3 Backtesting before release</a> |
| <a href="#ph-2">PH-2</a> | Train baseline probability + timing models | <a href="#f7">F-07 Backtesting Dashboard</a> | <a href="#s-b3">S-B3 Backtesting before release</a> |
| <a href="#ph-3">PH-3</a> | Deliver MVP API + UI for prediction flow | <a href="#f1">F-01 Stock Selection</a> | <a href="#s-a1">S-A1 Select a stock</a> |
| <a href="#ph-3">PH-3</a> | Deliver MVP API + UI for prediction flow | <a href="#f2">F-02 User Profit % Configuration</a> | <a href="#s-a2">S-A2 Set desired profit %</a> |
| <a href="#ph-3">PH-3</a> | Deliver MVP API + UI for prediction flow | <a href="#f3">F-03 Buy GTT Suggestion</a> | <a href="#s-a3">S-A3 Get suggested Buy GTT</a> |
| <a href="#ph-3">PH-3</a> | Deliver MVP API + UI for prediction flow | <a href="#f4">F-04 Sell GTT Target Prediction</a> | <a href="#s-a4">S-A4 Predict Sell GTT target timing</a> |
| <a href="#ph-3">PH-3</a> | Deliver MVP API + UI for prediction flow | <a href="#f5">F-05 Explanation Layer</a> | <a href="#s-a5">S-A5 Understand prediction confidence</a> |
| <a href="#ph-4">PH-4</a> | Add trust, backtesting, and observability for launch readiness | <a href="#f7">F-07 Backtesting Dashboard</a> | <a href="#s-b3">S-B3 Backtesting before release</a> |
| <a href="#ph-4">PH-4</a> | Add trust, backtesting, and observability for launch readiness | <a href="#f5">F-05 Explanation Layer</a> | <a href="#s-a5">S-A5 Understand prediction confidence</a> |
| <a href="#ph-5">PH-5</a> | Expand to personalization, alerts, and advanced modeling | <a href="#f8">F-08 Alerts and Notifications</a> | <a href="#s-c2">S-C2 Compare multiple profit % scenarios</a> |
| <a href="#ph-5">PH-5</a> | Expand to personalization, alerts, and advanced modeling | <a href="#f9">F-09 User Preferences</a> | <a href="#s-c1">S-C1 Save default profit %</a>, <a href="#s-c2">S-C2 Compare multiple profit % scenarios</a> |
| <a href="#ph-5">PH-5</a> | Expand to personalization, alerts, and advanced modeling | <a href="#f10">F-10 Watchlist Predictions</a> | <a href="#s-c2">S-C2 Compare multiple profit % scenarios</a> |
| <a href="#ph-5">PH-5</a> | Expand to personalization, alerts, and advanced modeling | <a href="#f11">F-11 Stop-loss and Risk/Reward Optimization</a> | <a href="#s-c2">S-C2 Compare multiple profit % scenarios</a> |
| <a href="#ph-5">PH-5</a> | Expand to personalization, alerts, and advanced modeling | <a href="#f12">F-12 Personalized Models</a> | <a href="#s-c1">S-C1 Save default profit %</a> |
| <a href="#ph-5">PH-5</a> | Expand to personalization, alerts, and advanced modeling | <a href="#f13">F-13 Event-aware Forecasting</a> | <a href="#s-c2">S-C2 Compare multiple profit % scenarios</a> |

### Feature and Backlog Traceability (Support View)

| Phase | Features (linked) | Backlog (linked) |
|---|---|---|
| Phase 0 (Week 1) | <a href="#f1">Stock Selection</a>, <a href="#f2">User Profit % Configuration</a>, <a href="#f3">Buy GTT Suggestion</a>, <a href="#f4">Sell GTT Target Prediction</a>, <a href="#f5">Explanation Layer</a> | <a href="#b-p0-product-data">P0 Product and Data Foundations</a> (scope, contract, labels, provider selection) |
| Phase 1 (Weeks 2-4) | <a href="#f6">Historical + Daily Data Pipeline</a>, <a href="#f1">Stock Selection</a> | <a href="#b-p0-product-data">P0 Product and Data Foundations</a> (schema, mapping, backfill, refresh, data quality) |
| Phase 2 (Weeks 4-7) | <a href="#f3">Buy GTT Suggestion</a>, <a href="#f4">Sell GTT Target Prediction</a>, <a href="#f6">Historical + Daily Data Pipeline</a>, <a href="#f7">Backtesting Dashboard</a> | <a href="#b-p0-modeling">P0 Modeling MVP</a> |
| Phase 3 (Weeks 7-10) | <a href="#f1">Stock Selection</a>, <a href="#f2">User Profit % Configuration</a>, <a href="#f3">Buy GTT Suggestion</a>, <a href="#f4">Sell GTT Target Prediction</a>, <a href="#f5">Explanation Layer</a> | <a href="#b-p0-api-ux">P0 API and UX MVP</a> |
| Phase 4 (Weeks 10-12) | <a href="#f7">Backtesting Dashboard</a>, <a href="#f5">Explanation Layer</a> | <a href="#b-p1-reliability">P1 Reliability and Observability</a> |
| Phase 5 (Post-MVP) | <a href="#f8">Alerts and Notifications</a>, <a href="#f9">User Preferences</a>, <a href="#f10">Watchlist Predictions</a>, <a href="#f11">Stop-loss and Risk/Reward Optimization</a>, <a href="#f12">Personalized Models</a>, <a href="#f13">Event-aware Forecasting</a> | <a href="#b-p1-ux-enhancements">P1 User Experience Enhancements</a>, <a href="#b-p2-advanced">P2 Advanced Features</a> |

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
### Feature F-01: Stock Selection (Indian Equities)
- Search/select stock by symbol and company name
- Support initial universe (e.g., NIFTY 50 / curated list)
- Display latest close and recent trend summary

<a id="f2"></a>
### Feature F-02: User Profit % Configuration
- User enters desired profit percentage per transaction
- Validation for min/max range (example: 1% to 25%)
- Profit % stored per prediction request and optionally as default preference

<a id="f3"></a>
### Feature F-03: Buy GTT Suggestion
- Suggest a Buy GTT trigger price based on candidate entry rules + model ranking
- Show confidence and expected fill window
- Show assumptions used (horizon, volatility regime)

<a id="f4"></a>
### Feature F-04: Sell GTT Target Prediction
- Compute sell target price from buy price and user profit %
- Predict probability of target hit within horizons (5/10/20 trading days)
- Predict expected time to target hit

<a id="f5"></a>
### Feature F-05: Explanation Layer (GenAI-assisted)
- Natural language explanation of prediction drivers
- Plain-language risk notes and confidence interpretation
- Explain how profit % affects probability/timing

<a id="f6"></a>
### Feature F-06: Historical + Daily Data Pipeline
- Historical backfill ingestion
- Daily incremental updates
- Data quality checks for missing/duplicate candles

<a id="f7"></a>
### Feature F-07: Backtesting Dashboard (Internal MVP)
- Hit-rate by stock and horizon
- Calibration checks (predicted vs actual)
- Performance by market regime/time window

## V1.1 Features (Should Have)

<a id="f8"></a>
### Feature F-08: Alerts and Notifications
- Notify users when predicted target window approaches
- Notify when confidence drops materially after market move

<a id="f9"></a>
### Feature F-09: User Preferences
- Default profit %
- Preferred horizon
- Risk appetite (conservative/balanced/aggressive)

<a id="f10"></a>
### Feature F-10: Watchlist Predictions
- Batch predictions for a user watchlist
- Rank opportunities by probability x expected time

## Future Features (Could Have)

<a id="f11"></a>
### Feature F-11: Stop-loss and Risk/Reward Optimization
- User-configurable stop-loss %
- Recommend trade only if risk/reward and probability criteria met

<a id="f12"></a>
### Feature F-12: Personalized Models
- Personalization based on user behavior, holding period, and risk profile

<a id="f13"></a>
### Feature F-13: Event-aware Forecasting
- Earnings calendar, corporate announcements, macro signals, news sentiment

## User Stories

## Epic A: Core Prediction Experience

<a id="s-a1"></a>
### Story S-A1: Select a stock
As a user, I want to search and select an Indian stock so that I can request a prediction for that stock.

Scope:
- Search by NSE/BSE symbol and company name for the supported stock universe.
- Return a selectable result with symbol, company name, exchange, and latest market date.

Dependencies:
- Feature: Stock Selection
- Data readiness from daily refresh/backfill (Stories B1, B2)

Acceptance Criteria:
- I can search by symbol or company name with partial input.
- The system shows matching supported stocks with no unsupported symbols in results.
- Selecting a stock loads a latest market snapshot (at minimum close price and as-of date).
- If fresh market data is unavailable, the UI shows the last available market date clearly.

Edge Cases:
- No results for query.
- Ambiguous symbols across exchanges.
- Market closed day / holiday (latest date is prior trading day).

<a id="s-a2"></a>
### Story S-A2: Set desired profit %
As a user, I want to enter my desired profit percentage for a transaction so that the prediction is tailored to my goal.

Scope:
- User enters and edits a profit target % as part of a prediction request.
- Input is validated before API submission.

Dependencies:
- Feature: User Profit % Configuration
- API request schema for `/predict_gtt`

Acceptance Criteria:
- I can enter a profit % value and submit a prediction request.
- Invalid inputs (empty, non-numeric, negative, out-of-range) are rejected with clear messages.
- The selected profit % is echoed in the prediction response.
- The sell target price is computed using the chosen profit % and displayed in the result.

Edge Cases:
- Decimal percentages (e.g., `4.5%`)
- Very high values above configured threshold
- Locale formatting input (e.g., `5`, `5%`, `5.0`)

<a id="s-a3"></a>
### Story S-A3: Get suggested Buy GTT
As a user, I want a suggested Buy GTT price so that I know a reasonable entry trigger to place.

Scope:
- API returns one ranked Buy GTT suggestion based on candidate generation and model scoring.
- UI displays the suggested trigger with context.

Dependencies:
- Feature: Buy GTT Suggestion
- Modeling outputs from Phase 2
- Input stock selection and profit % (Stories A1, A2)

Acceptance Criteria:
- The system returns a suggested Buy GTT price for supported symbols.
- Response includes confidence level and prediction timestamp.
- Response includes a short machine-readable explanation payload (for UI/GenAI explanation).
- UI shows the buy trigger and indicates the assumption horizon used.

Edge Cases:
- Low-confidence prediction (still returns result with warning)
- No valid candidate entry found (graceful fallback message)
- Model unavailable (degraded response or error state)

<a id="s-a4"></a>
### Story S-A4: Predict Sell GTT target trigger timing
As a user, I want to know the probability and expected time for my sell target (based on profit %) to be hit so that I can plan exits.

Scope:
- Compute sell target from buy GTT + user `profit_pct`.
- Predict probability and expected time-to-hit across one or more horizons.

Dependencies:
- Feature: Sell GTT Target Prediction
- Baseline probability and timing models from Phase 2

Acceptance Criteria:
- The system returns computed sell target price.
- The system returns hit probability for at least one horizon (target: 5/10/20 trading days).
- The system returns expected trading days to hit, or a clear “insufficient confidence” state.
- All values include an `as_of_date` and model version metadata in the API response.

Edge Cases:
- Probability available but no stable expected-days estimate
- Extremely low probability targets (e.g., high profit % in low-volatility stock)
- Stale feature data / data freshness warning

<a id="s-a5"></a>
### Story S-A5: Understand prediction confidence
As a user, I want the predictor to explain why confidence is high or low so that I can make informed decisions.

Scope:
- Show a concise explanation of confidence using model outputs and/or Vertex explanation layer.
- Explain tradeoffs (profit target vs probability/time).

Dependencies:
- Feature: Explanation Layer
- Prediction output from Stories A3 and A4
- Vertex integration (optional in MVP, fallback supported)

Acceptance Criteria:
- Explanation is plain language and understandable.
- Explanation references key drivers (e.g., trend, volatility, volume, horizon).
- Explanation clarifies confidence level meaning (high/medium/low).
- UI includes a non-advisory disclaimer.
- If explanation generation fails, prediction still renders with a fallback message.

Edge Cases:
- Vertex timeout/error
- Missing driver summary data from model response
- User requests unsupported language (future enhancement)

## Epic B: Data and Reliability

<a id="s-b1"></a>
### Story S-B1: Daily market data refresh
As a platform operator, I want daily stock data to refresh automatically so that predictions use recent data.

Scope:
- Scheduled ingestion of daily OHLCV data for the supported universe.
- Data quality checks and run-status logging.

Dependencies:
- Feature: Historical + Daily Data Pipeline
- Scheduler setup (GitHub Actions / cron)
- Data provider integration

Acceptance Criteria:
- A scheduled job ingests the latest daily candles for supported stocks.
- Missing and duplicate rows are detected and flagged.
- Run status (success/failure, processed count, timestamp) is visible in logs/monitoring.
- Pipeline is idempotent for repeated runs on the same market date.

Edge Cases:
- Exchange holiday / no trading session
- Partial provider outage
- Late-arriving data corrections

<a id="s-b2"></a>
### Story S-B2: Historical backfill for supported stocks
As a platform operator, I want historical data backfilled so that the model can train on sufficient history.

Scope:
- Backfill historical OHLCV data across the chosen stock universe and training time range.
- Generate a coverage summary for model readiness.

Dependencies:
- Feature: Historical + Daily Data Pipeline
- Data provider integration
- Symbol normalization logic

Acceptance Criteria:
- Backfill runs for the supported stock universe across the configured history window.
- Corporate action handling (adjusted data or documented limitations) is implemented or explicitly documented.
- A data coverage report is generated (symbols, date range, gaps, row counts).
- Backfill can be rerun safely without duplicating rows.

Edge Cases:
- Symbol rename/migration
- Missing early history for some stocks
- Provider returns inconsistent date formats/timezones

<a id="s-b3"></a>
### Story S-B3: Backtesting before release
As a platform operator, I want walk-forward backtests so that we can validate the predictor before user launch.

Scope:
- Run walk-forward evaluation on historical periods before release decisions.
- Produce metrics suitable for launch go/no-go.

Dependencies:
- Feature: Backtesting Dashboard
- Modeling outputs and saved artifacts
- Label generation + feature pipeline stability

Acceptance Criteria:
- Backtests avoid future leakage (time-based train/test split and walk-forward evaluation).
- Metrics are generated by time period and stock (at minimum hit rate and timing error).
- Calibration metrics are included for probability outputs.
- A report artifact is generated and versioned for review.

Edge Cases:
- Regime shift periods (bull/bear/sideways)
- Sparse data for newer symbols
- Metrics degradation after retraining compared to previous version

## Epic C: User Preferences and UX

<a id="s-c1"></a>
### Story S-C1: Save default profit %
As a returning user, I want to save a default profit % so that I do not need to re-enter it each time.

Scope:
- Persist a user-level default profit % preference and prefill it in the prediction form.

Dependencies:
- Feature: User Preferences
- User identity/session model (or local storage in MVP fallback)

Acceptance Criteria:
- User can set and update default profit %.
- New prediction forms prefill the saved value.
- Manual override for an individual prediction request is allowed without changing the saved default.
- The saved default is validated using the same rules as Story A2.

Edge Cases:
- No saved preference exists
- Invalid legacy saved value after validation rule changes
- Anonymous user mode (fallback local preference)

<a id="s-c2"></a>
### Story S-C2: Compare multiple profit % scenarios
As a user, I want to compare outcomes for different profit % values so that I can choose a realistic target.

Scope:
- Run and display multiple prediction scenarios for the same stock across multiple profit % values.

Dependencies:
- Feature: Watchlist Predictions (batching pattern, optional reuse)
- Feature: User Preferences
- Stories A3, A4, A5 baseline prediction/explanation flow

Acceptance Criteria:
- User can request at least 2-3 scenarios (e.g., 4%, 8%, 12%).
- UI shows side-by-side comparison of target price, hit probability, and expected time.
- Explanation highlights the tradeoff between higher profit and lower hit probability.
- Scenario results share the same as-of date or clearly show if they differ.

Edge Cases:
- One scenario returns low confidence while others succeed
- API timeout for one scenario in a batch
- Duplicate profit % values entered by user

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
