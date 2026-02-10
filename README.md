# Mini BI Backend (Power BI-style MVP)

This repository contains a backend MVP for a mini Power BI-like workflow:

1. Upload CSV/Excel
2. Auto-analyze data with a Python "AI analyst" layer
3. Generate dashboard-ready visualization specs
4. Chat with an AI advisor over the uploaded dataset
5. Export analysis with free vs pro subscription rules

## Tech stack
- FastAPI
- Pandas
- Uvicorn
- Pydantic

## Quick start
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Open docs at `http://127.0.0.1:8000/docs`.

## API overview
- `POST /v1/auth/demo-login` – create/get demo user and set plan (`free` or `pro`)
- `POST /v1/data/upload` – upload a CSV/XLSX and auto-analyze
- `GET /v1/data/{dataset_id}/dashboard` – get generated dashboard payload
- `POST /v1/advisor/chat` – ask dataset-aware AI advisor questions
- `GET /v1/data/{dataset_id}/export?format=csv|json|pdf&user_id=...` – export with plan checks
- `GET /v1/users/{user_id}/usage` – see limits/usage

## Subscription model (MVP)
- **Free**
  - Max 1,500 rows analyzed per upload
  - JSON export only
- **Pro**
  - Up to 150,000 rows analyzed per upload
  - JSON/CSV/PDF export

## Notes
- LLM calls are implemented as a pluggable layer. By default, this MVP uses deterministic heuristics for reliability and low cost.
- Data is stored in-memory for simplicity. Replace `InMemoryStore` with Redis/Postgres/S3 for production.
