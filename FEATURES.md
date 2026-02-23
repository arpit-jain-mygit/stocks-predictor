# Features

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
