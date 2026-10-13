# RentVerse — AI Service & Data Pipeline

Machine-learning components of **RentVerse**, a secure property rental platform for Malaysia built as a group project. This repository holds the data pipeline that collects rental listings and the FastAPI service that predicts rent and screens new listings.

## Results

Rent-price models compared on held-out test data (from [`notebooks/`](rentverse-ai-service-main/notebooks)):

| Model | Test R² | Test MAE (RM) | Test MAPE |
| --- | --- | --- | --- |
| **Extra Trees (log-transformed target)** | **0.84** | **166** | **11.0%** |
| Gradient Boosting | 0.83 | 189 | 12.5% |
| Random Forest | 0.72 | 252 | 16.7% |
| Ridge Regression | 0.33 | 462 | 33.4% |
| Linear Regression | 0.33 | 462 | 33.4% |

The Extra Trees pipeline is the one deployed in the API.

## What's inside

| Folder | What it does |
| --- | --- |
| [`rentverse-datasets-main/`](rentverse-datasets-main) | Scrapy spider that crawls FazWaz rental listings across Malaysian states and exports a cleaned CSV |
| [`rentverse-ai-service-main/notebooks/`](rentverse-ai-service-main/notebooks) | Feature engineering, model training and comparison (Jupyter) |
| [`rentverse-ai-service-main/rentverse/`](rentverse-ai-service-main/rentverse) | FastAPI service that serves the trained models |

## API

Base path: `/api/v1`. Interactive docs at `/docs` once running.

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | `/health/` | Service health check |
| GET | `/health/model` | Loaded model information |
| POST | `/predict/single` | Predict rent for one property |
| POST | `/predict/batch` | Predict rent for many properties |
| POST | `/classify/price` | Simple price prediction |
| POST | `/classify/approval` | Approve, review or reject a new listing based on its price against the market |

## Tech stack

Python 3.12 · scikit-learn · pandas · NumPy · FastAPI · Pydantic · Scrapy · Poetry · Docker

## Running it

**Scrape data**

```bash
cd rentverse-datasets-main
poetry install
poetry run scrapy crawl fazwazrent -a region=penang -a property_type=house
```

**Start the API**

```bash
cd rentverse-ai-service-main
poetry install
poetry run dev          # http://localhost:8000/docs
# or with Docker
docker compose up --build
```

See the [AI service README](rentverse-ai-service-main/README.md) for full endpoint examples.

## My role

Group project. I contributed across the data scraping, model training and FastAPI service in this repo, as well as the platform's Next.js frontend and the backend security hardening (JWT, RBAC, Helmet.js, strict CORS, parameterised Prisma queries, and an `npm audit` check in GitHub Actions).
