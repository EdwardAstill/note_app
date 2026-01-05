## Step 2 — Backend (FastAPI + SQLite)

### Goal
Expose a tiny JSON API that supports: list notes, create note, get note, delete note.

### Files (suggested)
- `backend/app/main.py` (FastAPI app + routes)
- `backend/app/models.py` (Pydantic models)
- `backend/app/db.py` (SQLite init + queries)
- `backend/requirements.txt`

### Data model
Table `notes`:
- `id` TEXT PRIMARY KEY (uuid string)
- `title` TEXT NOT NULL
- `created_at` TEXT NOT NULL (ISO)

### Endpoints
- `GET /health` → `{ "status": "ok" }`
- `GET /notes` → list notes (id, title, created_at)
- `POST /notes` → create note from `{ "title": "..." }`
- `GET /notes/{id}` → return one note
- `DELETE /notes/{id}` → delete note

### Rules
- Return **404** if `{id}` doesn’t exist.
- Enable CORS for the Vite dev origin (`http://localhost:5173`).

### Commands
From repo root:
- Create venv + install deps
- Run server:
  - `uvicorn app.main:app --reload --port 8000`

### Quick manual test (curl)
- `GET http://localhost:8000/health`
- `POST http://localhost:8000/notes` with JSON body `{ "title": "Test" }`
- `GET http://localhost:8000/notes`
- `DELETE http://localhost:8000/notes/{id}`


