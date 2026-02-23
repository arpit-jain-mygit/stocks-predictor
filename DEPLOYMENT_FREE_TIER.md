# Deployment (Free-Tier MVP)

## Goal

Deploy the stock predictor MVP using mostly free-tier services while keeping the system easy to evolve later.

## Chosen Hosting

- UI: Vercel (Hobby)
- API: Render (Free Web Service)
- GenAI: GCP Vertex AI (using existing GCP credits)

## Recommended Remaining Stack

### Database (Primary)
Option A (Recommended): Neon Free (Postgres)
- Use for users, preferences, prediction requests, prediction results metadata, model versions.
- Good fit with Render and easy later migration/upgrade.

Option B: Supabase Free
- Use if you also want built-in auth/storage quickly.

### Cache / Lightweight Queue
- Upstash Redis Free
- Use for:
  - caching prediction responses
  - caching Vertex explanations
  - simple rate limiting
  - lightweight job state (if needed)

### Object Storage (Data + Model Artifacts)
Option A (Recommended): Cloudflare R2
- Store historical data snapshots, feature exports, model artifacts, backtest reports.

Option B: Supabase Storage
- Simpler if using Supabase already.

### Scheduler / Cron
Option A (Recommended): GitHub Actions (scheduled workflows)
- Daily data refresh
- Weekly retraining
- Nightly health checks

Option B: Vercel Cron
- Trigger API endpoints for lightweight scheduled tasks
- Keep heavy jobs out of Vercel functions

## MVP Environment Mapping

### Vercel (UI)
Responsibilities:
- host frontend
- call Render API
- display prediction outputs and explanations

Environment variables (UI):
- `NEXT_PUBLIC_API_BASE_URL` or `VITE_API_BASE_URL`

### Render (API)
Responsibilities:
- FastAPI inference endpoints
- request validation
- read model artifacts / feature config
- call Vertex for explanations
- persist prediction metadata to Postgres
- cache with Redis

Environment variables (API):
- `DATABASE_URL`
- `REDIS_URL`
- `VERTEX_PROJECT_ID`
- `VERTEX_LOCATION`
- `VERTEX_MODEL_NAME`
- `GCP_SERVICE_ACCOUNT_JSON` (or mounted secret file path)
- `MODEL_ARTIFACT_URI`
- `APP_ENV`
- `LOG_LEVEL`

### GCP Vertex AI
Responsibilities:
- natural-language explanations of model output
- user-facing Q&A and context summaries

### GitHub Actions
Responsibilities:
- daily ingestion/recompute jobs
- weekly retraining / backtesting jobs
- optional deploy hooks or smoke tests

## Suggested MVP Deployment Flow

1. User opens UI on Vercel and submits `symbol + profit_pct + horizon`.
2. UI calls Render API `/predict_gtt`.
3. Render API:
   - reads/caches market features
   - runs prediction model
   - computes buy GTT + sell target + probabilities
   - optionally calls Vertex for explanation
   - stores metadata in Postgres
   - caches response in Redis
4. Render returns final payload to Vercel UI.
5. UI renders probability, expected days, confidence, disclaimer.

## Free-Tier Constraints to Design Around

### Render Free Web Service
- Idle spin-down and cold starts
- Use response caching and simple health warm-up if needed
- Keep API startup lightweight

### Vercel Hobby
- Avoid heavy compute in frontend/serverless functions
- Keep data/ML logic in Render API

### GitHub Actions
- Good for scheduled jobs; watch monthly usage limits
- Prefer incremental refresh over full reprocessing

### Vertex AI (Credits)
- Credit-backed, not truly free forever
- Add budgets, quotas, and caching from day one

## Cost Guardrails (MVP)

- Cache prediction explanations (`symbol + profit_pct + horizon + date` key)
- Use lower-cost/faster Vertex model for explanations first
- Truncate prompt context (only include compact model outputs/features)
- Add request rate limits per user/IP
- Disable explanations via feature flag if cost spikes (`ENABLE_VERTEX_EXPLANATIONS=false`)
- Run retraining weekly (not daily) in early MVP

## Suggested Folder/Service Ownership

- `web/`: Vercel app
- `api/`: Render FastAPI app
- `ml/`: training/inference code used by API and scheduled jobs
- `scripts/`: local + CI/CD scripts for refresh/retrain/backfill
- `docs/`: ops and runbooks

## MVP Rollout Plan (Infra)

1. Deploy API to Render with `/health` only.
2. Deploy UI to Vercel wired to Render base URL.
3. Add Neon + Upstash and persist prediction request logs.
4. Add Vertex explanation endpoint with caching.
5. Add GitHub Actions daily refresh job.
6. Add weekly retraining + backtest report generation.
