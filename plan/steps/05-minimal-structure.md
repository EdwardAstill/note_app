## Step 5 — Minimal repo structure (recommended)

### Goal
Keep the repo easy to navigate with a clear split between backend and frontend.

### Suggested layout
- `backend/`
  - `app/`
    - `main.py` (FastAPI app)
    - `db.py` (SQLite connection + queries)
    - `models.py` (Pydantic request/response models)
  - `requirements.txt`
  - `README.md` (how to run backend)
- `frontend/`
  - `src/` (React app)
  - `package.json`
  - `README.md` (how to run frontend)

### Notes
- Keep “fancy” features out until MVP is stable.
- After MVP, the easiest upgrade is adding an optional `content` field and a `PUT /notes/{id}` endpoint.


