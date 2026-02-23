# Stocks Predictor

A product and engineering repository for building a GenAI-assisted predictor for Indian stocks that helps users:

- get a suggested next Buy GTT trigger
- set a desired profit % per transaction
- estimate when a Sell GTT target is likely to trigger

This system is designed as a probabilistic prediction platform (not guaranteed financial advice) using historical and daily market data.

## Vision

Build a user-facing decision support system that combines:

- quantitative forecasting models for target-hit probability and time-to-target
- explainable insights for users via a GenAI layer
- configurable user profit goals and trading horizons

## Core MVP Outcome

For a selected stock and user-configured `profit_pct`, show:

- suggested Buy GTT price
- suggested Sell GTT target price
- probability target is hit within a selected horizon (e.g., 5/10/20 trading days)
- expected number of trading days to target
- confidence level and explanation

## Repo Planning Docs

- `PRODUCT_PLAN.md` - consolidated roadmap, features, user stories, and backlog

## Guiding Principles

- Probabilistic outputs over deterministic price claims
- No data leakage in training/backtesting
- User-configurable profit % as a first-class model input
- Explainability and risk disclosure in the UI
- India market specific support (NSE/BSE symbols, holidays, trading sessions)
# stocks-predictor
