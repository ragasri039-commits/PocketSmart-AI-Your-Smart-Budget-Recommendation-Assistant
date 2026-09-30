# PocketSmart AI — World-Class Full v2

PocketSmart AI is a budget-first planning platform for **Home Interior**, **Party**, and **Jewelry** decisions. It combines deterministic budget allocation, a bounded catalog, optional Gemini multimodal reasoning, authentication, saved history, and a premium responsive interface.

> **Important:** the included catalog is demo data. It is not live inventory and must not be presented as live price/availability data. Production commerce integrations require approved APIs, affiliate feeds, or other permitted data sources.

## What is included

### Product
- Premium marketing landing page
- Authenticated workspace/dashboard
- Home Interior Planner
- Party Planner
- Jewelry Planner with optional outfit image
- AI-generated structured plans
- Deterministic budget allocation + post-AI budget guardrail
- Saved recommendation history
- Open/delete saved plans
- Dashboard statistics
- Demo catalog API
- Mobile-first responsive navigation
- PWA manifest + service worker shell
- Accessible labels, focus states, responsive layouts

### Engineering
- FastAPI API layer
- SQLAlchemy models
- SQLite local database; PostgreSQL-ready database URL
- JWT authentication
- Argon2 password hashing
- Pydantic validation
- Gemini service isolated behind a service boundary
- Catalog service isolated behind a service boundary
- Modular planner service
- CORS configuration
- Health/readiness endpoints
- Docker + Docker Compose
- Pytest test suite
- Architecture/API documentation

## Architecture

```text
Browser / PWA
    |
    v
FastAPI routes
    |
    +--> Auth service ------> SQLAlchemy ------> SQLite/PostgreSQL
    |
    +--> Planner service
            |
            +--> Budget engine (deterministic)
            +--> Catalog provider (demo now; approved live adapters later)
            +--> Gemini service (optional)
            |
            v
        Validated PlanResult
            |
            v
        Saved Plan / UI
```

### Core rule
The model is **not** the source of truth for product identity, price, URL, or availability. The application supplies bounded catalog records, validates returned product names/prices against that catalog, and recomputes the estimated total on the server.

## Run locally

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate
pip install -r requirements.txt
copy .env.example .env
uvicorn app.main:app --reload
```

Open `http://127.0.0.1:8000`.

API docs: `http://127.0.0.1:8000/docs`

## Gemini

Set:

```env
GEMINI_API_KEY=your_key_here
GEMINI_MODEL=gemini-3.5-flash-lite
```

The model name is configurable. If no API key is configured, PocketSmart automatically uses its deterministic bounded-catalog fallback so the project can still be demonstrated locally.

## Docker

```bash
copy .env.example .env
docker compose up --build
```

## Tests

```bash
pytest -q
```

## Production hardening still required

This repository is a complete application foundation and demonstration build, but a real public launch still needs deployment-specific work:

1. PostgreSQL in production
2. Alembic migrations
3. Redis-backed rate limiting and caching
4. Object storage for uploaded outfit images if images must be retained
5. Approved live product/service data providers
6. Price/availability timestamps and freshness rules
7. Background jobs for large catalog synchronization
8. Centralized logging, metrics and tracing
9. HTTPS, secure cookie/session strategy and secrets manager
10. Account recovery, email verification and abuse protection
11. Privacy policy, terms, AI disclosure and affiliate disclosures where applicable
12. Automated CI/CD and dependency/security scanning
13. Real device/browser QA

## Product roadmap

### Phase A — complete demo
- Home, Party, Jewelry planners
- Auth
- Saved history
- Gemini structured generation
- Demo catalog

### Phase B — live recommendation infrastructure
- Provider adapter interface
- Amazon/Flipkart/IKEA/etc. approved feeds/APIs where available
- Search, ranking and freshness layer
- Affiliate attribution
- Availability checks

### Phase C — production platform
- PostgreSQL + Redis
- Object storage
- Observability
- CI/CD
- Account recovery and verification
- Privacy/security hardening
- Scalable deployment

## Safety / accuracy

PocketSmart is a planning assistant. It does not guarantee product quality, price, inventory, delivery, vendor performance or financial outcomes. Users should verify live details on the destination platform before purchasing or booking.
