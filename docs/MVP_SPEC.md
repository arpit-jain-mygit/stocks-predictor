# MVP Spec (Draft)

## Primary API Use Case

Given `symbol`, `profit_pct`, and optional `horizon_days`, return a suggested Buy GTT and predicted Sell GTT target-hit probability/timing.

## Draft Response Fields

- `symbol`
- `as_of_date`
- `buy_gtt_suggestion`
- `sell_target_price`
- `profit_pct`
- `horizon_probabilities` (e.g. 5/10/20 days)
- `expected_days_to_target`
- `confidence`
- `explanation_summary`
- `model_version`
