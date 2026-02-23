# Backlog

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
