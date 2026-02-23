# Vertex AI Integration (GenAI Explanation Layer)

## Purpose

Use GCP Vertex AI to generate plain-language explanations and Q&A for prediction outputs.

Important:
- Vertex is an explanation layer.
- The core Buy GTT / Sell GTT timing prediction should come from deterministic ML models (e.g., XGBoost/LightGBM).

## What Vertex Should Do (MVP)

### 1. Prediction Explanation
Convert structured prediction output into user-friendly text, for example:
- why confidence is low/high
- why a higher `profit_pct` reduces hit probability
- expected timing tradeoffs

### 2. User Q&A
Answer natural language questions based on returned prediction payload, for example:
- “Why is 12% less likely than 6%?”
- “What does medium confidence mean?”

### 3. Risk Messaging
Generate standardized but personalized risk explanations and disclaimers.

## What Vertex Should NOT Do (MVP)

- Predict raw next-day stock prices directly
- Replace model scoring logic
- Operate on full historical candle data in prompts (too expensive and noisy)

## Integration Architecture

### API-Centric Flow (Recommended)

1. Client calls Render API `/predict_gtt`.
2. API computes prediction using local model artifacts.
3. API builds a compact explanation input object.
4. API calls Vertex AI for explanation text (optional / feature-flagged).
5. API returns combined response to client.

## Minimal Structured Input to Vertex

Send compact structured fields, not raw time series. Example:

- `symbol`
- `as_of_date`
- `buy_gtt_suggestion`
- `profit_pct`
- `sell_target_price`
- `prob_hit_5d`
- `prob_hit_10d`
- `prob_hit_20d`
- `expected_days_to_target`
- `confidence`
- `drivers_summary` (top factors from model, short list)
- `risk_flags` (e.g., high volatility, weak trend)

## Prompting Strategy (MVP)

Use a strict system prompt that enforces:
- no guaranteed claims
- no financial-advice language
- plain English explanations
- concise output format
- use only supplied data

### Recommended Response Structure

- `summary` (2-4 lines)
- `why_this_prediction`
- `profit_target_tradeoff`
- `confidence_note`
- `risk_note`
- `disclaimer`

## Cost and Latency Controls (Important)

### 1. Feature Flagging
- `ENABLE_VERTEX_EXPLANATIONS=true|false`
- `ENABLE_VERTEX_QA=true|false`

### 2. Caching
Cache explanations by key:
- `symbol`
- `profit_pct`
- `horizon`
- `as_of_date`
- `model_version`

This avoids repeated generation for identical inputs.

### 3. Prompt Size Limits
- Send only top N drivers (e.g., 3-5)
- Avoid raw candle arrays
- Avoid verbose system/user history for MVP

### 4. Model Selection
- Start with a lower-cost, fast model for explanations
- Upgrade only if quality is insufficient

### 5. Timeout + Fallback
- Vertex timeout (e.g., 2-4s)
- If timeout/error, return prediction without explanation
- UI should handle `explanation_summary = null`

## Security and Credentials

### Service Account (Recommended)
Use a dedicated GCP service account with least privilege for Vertex calls.

Suggested approach on Render:
- Store service account JSON as a secret env var (`GCP_SERVICE_ACCOUNT_JSON`)
- Load credentials at app startup and initialize Vertex client

Alternative:
- Mount secret file (if deployment setup supports it)

### Required GCP Config
- `VERTEX_PROJECT_ID`
- `VERTEX_LOCATION` (e.g., `asia-south1` or your chosen region)
- `VERTEX_MODEL_NAME`

## API Contract Suggestion

### `/predict_gtt` response (partial)
- `prediction`: structured ML output
- `explanation_summary`: string or object (nullable)
- `explanation_status`: `generated | cached | skipped | failed`

### `/ask_prediction` (optional later)
- Input: prediction payload ID + user question
- Output: constrained Q&A answer grounded in prediction payload only

## Failure Handling

If Vertex fails:
- Log error with request ID (without sensitive payloads)
- Return prediction response without explanation
- Set `explanation_status = failed`
- Do not fail the whole user request unless explanation is a hard requirement

## Observability

Track:
- Vertex call count/day
- cache hit rate
- average latency
- timeout/error rate
- cost estimate by endpoint (if available via billing export later)

## Compliance / User Messaging (MVP)

Always include user-facing language that:
- predictions are probabilistic
- not guaranteed outcomes
- not investment advice
- market conditions can change quickly

## Example Implementation Plan (Phased)

1. Add feature flag and placeholder explanation field.
2. Add Vertex client wrapper in API.
3. Add explanation cache in Redis.
4. Add timeout/fallback behavior.
5. Add basic prompt templates and response parser.
6. Add Q&A endpoint only after core prediction UX is stable.
