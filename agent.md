# Agent Instructions — Financial Research Analyst Agent

## Purpose

This file provides repository guidance for the Financial Research Analyst Agent. It is intended for AI coding agents and developers working in this codebase. Follow the repo’s architecture and conventions when making changes.

## Repository

- Repository: `gsaini/financial-research-analyst-agent`
- Project: AI-powered autonomous financial research and investment insight platform
- Language: Python
- Minimum Python version: 3.14+
- Default branch: `main`

## Product Overview

This project is a hierarchical multi-agent system for financial data analysis and investment insight generation. It coordinates specialized agents that collect market data, analyze multi-dimensional signals, and generate research reports. The app is built for local experimentation and deployment via API, CLI, Streamlit dashboard, Docker, and Kubernetes.

The system is intended for research and analysis workflows, not as guaranteed financial advice. Preserve confidence scores, reasoning, and disclosures in generated reports.

## High-Level Architecture

```text
FinancialResearchAgent
        |
        v
OrchestratorAgent
        |
        +--> DataCollector
        +--> TechnicalAnalyst
        +--> FundamentalAnalyst
        +--> SentimentAnalyst
        +--> RiskAnalyst
        +--> ThematicAnalyst
        +--> DisruptionAnalyst
        +--> EarningsAnalyst
        +--> PerformanceAnalyst
        +--> ReportGenerator
        |
        +--> RAG/document intelligence
        +--> LLM synthesis and conflict detection
```

## Key Responsibilities by Agent

| Agent | Responsibility |
|---|---|
| `OrchestratorAgent` | Coordinates workflow, delegates tasks, aggregates results, detects conflicts |
| `DataCollector` | Retrieves price, statement, news, and market data from providers |
| `TechnicalAnalyst` | RSI, MACD, moving averages, Bollinger Bands, regime detection, forecasting |
| `FundamentalAnalyst` | P/E, ROE, margins, valuation, DCF, peer context |
| `SentimentAnalyst` | News and social sentiment, transcript tone, analyst and insider signals |
| `RiskAnalyst` | Volatility, VaR/CVaR, Sharpe/Sortino, beta, drawdown, scenario risk |
| `ThematicAnalyst` | Theme-to-ticker mapping, momentum, diversification, sector overlap |
| `DisruptionAnalyst` | Innovation intensity, growth acceleration, margins, disruption scoring |
| `EarningsAnalyst` | EPS surprises, beat/miss patterns, earnings quality |
| `PerformanceAnalyst` | Multi-horizon returns, benchmark comparisons, alpha/beta |
| `ReportGenerator` | Executive summary, JSON/Markdown/PDF/Excel reports |

## Repository Layout

```text
src/
  agents/          Agent implementations and orchestration
  api/             FastAPI routes and request/response schemas
  models/          Pydantic data models
  rag/             Document ingestion, embedding, retrieval
  tools/           Data fetching, metrics calculations, models, alerts, reports
  utils/           Shared helpers and logging
  main.py          App entry point
  cli.py           CLI
  config.py        Typed settings
frontend/          Streamlit dashboard
static/            Static dashboard assets
config/            YAML configuration for agents/themes
data/              Local data, sample data, database files, vector store persistence
docs/              Project docs
notebooks/         Exploratory notebooks
postman/           API samples/collections
k8s/               Kubernetes manifests
tests/             pytest suite
Dockerfile         Container build
requirements.txt   Core backend dependencies
frontend/requirements.txt  Dashboard dependencies
.env.example       Template for environment variables
README.md         Main project documentation
CLAUDE.md         Claude-specific guidance
CONTRIBUTING.md    Contribution guidelines
```

## Important Modules

- `src/agents/base.py` — shared agent abstraction, LLM init, tool binding, state tracking, async/sync execution
- `src/agents/orchestrator.py` — orchestrates specialized agents and aggregates results
- `src/agents/rag_mixin.py` — reusable RAG-aware agent behavior
- `src/tools/` — financial data connectors, calculations, forecasting models, backtesting, alerts, reports, document search
- `src/config.py` — typed environment/settings model
- `config/agents.yaml` — configuration for recommendation thresholds, indicators, weights, providers, and defaults

## Stack

- Python
- LangChain / LangGraph
- FastAPI
- Streamlit
- PostgreSQL / SQLite
- Redis
- ChromaDB / vector search
- Pandas, NumPy, SciPy
- scikit-learn
- Plotly, reportlab, openpyxl
- Ollama / OpenAI / Groq / Anthropic support

## Setup

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

For frontend dependencies:

```bash
pip install -r frontend/requirements.txt
```

Optional: install Ollama models for local LLM use.

```bash
ollama pull llama4:latest
```

## Run Commands

### API

```bash
python -m src.main api
```

### CLI

```bash
python -m src.cli analyze AAPL
python -m src.cli portfolio AAPL GOOGL MSFT
python -m src.cli dashboard --port 8080
```

### Streamlit

```bash
streamlit run frontend/app.py
```

### Docker

```bash
docker-compose up -d
```

### Kubernetes

```bash
kubectl apply -f k8s/
```

## Configuration

Copy `.env.example` to `.env` and fill in real secrets. Never commit `.env` files or API keys.

Key settings include:

- LLM provider and model
- Embedding provider/model
- Data provider and fallback provider
- `DATABASE_URL`, `REDIS_URL`, and vector-store settings
- `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `FRED_API_KEY`, etc.
- API host/port, debug, logging, rate limiting

The default config prefers open-source/local-first tooling such as Ollama and ChromaDB, with optional commercial providers.

## Coding Conventions

1. Keep changes narrow and aligned with the agent/tool layering.
2. Prefer adding logic to existing abstractions rather than creating new ad hoc patterns.
3. Use the repo’s typed settings and environment configuration rather than hardcoded values.
4. Keep agent orchestration and tool logic separate.
5. Preserve provider fallbacks and error handling.
6. Test live-provider behavior with mocks or deterministic fixtures.
7. Do not remove disclaimers or confidence metadata from generated output.
8. Maintain backward compatibility unless the change is explicitly intended as a breaking update.

## Testing

```bash
pytest tests/ -v
pytest tests/test_agents.py -v
pytest tests/ --cov=src --cov-report=html
```

When adding or changing functionality:

- add or update unit tests under `tests/`
- mock external API calls and network dependencies
- validate both success and failure states
- test config changes and fallback logic

## Formatting and Quality Checks

```bash
black src/ tests/
isort src/ tests/
flake8 src/ tests/
mypy src/
```

## Security Notes

- Never commit secrets, tokens, or `.env` files.
- Treat all external financial/news sources as untrusted inputs.
- Use environment variables and secret management for credentials.
- Preserve API key checks, rate limiting, sanitization, and CORS rules.

## Definition of Done

A change is ready when:

- it follows the repo’s architecture and conventions,
- tests cover the new or changed behavior,
- relevant checks pass,
- docs/config are updated when necessary,
- no secrets or temporary files are left behind.

## Short Workflow for New Tasks

1. Read the relevant agent, tool, and config files.
2. Make the smallest correct change.
3. Add tests for changed behavior.
4. Validate with focused tests.
5. Update docs or `.env.example` when behavior/config changes.
6. Review the diff for security, compatibility, and scope issues.

This repository is an AI financial research platform for autonomous stock analysis, portfolio evaluation, theme exploration, disruption analysis, earnings analysis, and investment reporting. Keep changes aligned with that mission and maintain the repo’s local-first, multi-agent structure.
