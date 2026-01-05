## `main.py` — FastAPI application & routes

### Purpose
`main.py` defines the FastAPI app, configures CORS (cross-origin resource sharing) for the frontend, initializes the database on startup, and exposes the note endpoints.

### Lifespan startup (`@asynccontextmanager` / `lifespan`)
In your code:
- the `lifespan()` function is an **application lifecycle hook**
- it runs **once when the app starts**, and (optionally) once when it shuts down

Specifically, this block:
- `@asynccontextmanager` turns the function into a context manager FastAPI can use for lifecycle events
- `init_db()` runs **before** `yield` (startup)
- the `yield` line hands control back to FastAPI so it can start serving requests

This replaces the older `@app.on_event("startup")` approach, which FastAPI has deprecated.

### CORS (dev)
CORS middleware is enabled with:
- **Allowed origin**: `http://localhost:5173` (Vite dev server)
- **Allowed methods**: `GET`, `POST`, `DELETE`
- **Allowed headers**: `Content-Type`

If your frontend runs on a different port, you must update `allow_origins`.

### App creation + middleware (`FastAPI(...)` and `app.add_middleware(...)`)
This section:
- `app = FastAPI(lifespan=lifespan)` creates the FastAPI application object and tells it to use your `lifespan()` lifecycle hook.
- `app.add_middleware(CORSMiddleware, ...)` adds CORS rules so the browser will allow your React app (running on `localhost:5173`) to call the API.

Without CORS, your frontend would typically fail with a browser error like “blocked by CORS policy”.

### Endpoints
### What the `@app.get(...)` / `@app.post(...)` lines mean
Lines like:
- `@app.get("/health", response_model=HealthOut)`

are **decorators**. They tell FastAPI:
- “register the function right below this as a handler for HTTP requests”
- which **HTTP method** and **path** it handles (GET `/health`)
- what **shape** the response should have (`response_model=...`)

When you start uvicorn, FastAPI scans these decorators and builds the routing table.

- **`GET /health`**
  - Response: `{ "status": "ok" }`

- **`GET /notes`**
  - Response: list of notes (newest first)
  - Each item: `{ "id": "...", "title": "...", "created_at": "..." }`

- **`POST /notes`**
  - Body: `{ "title": "..." }`
  - Validation: title must be 1–200 characters
  - Creates:
    - `id` as a UUID string
    - `created_at` as UTC ISO time (`datetime.now(timezone.utc).isoformat()`)
  - Response: created note as `NoteOut`

- **`GET /notes/{note_id}`**
  - Response: note as `NoteOut`
  - Errors:
    - `404` if note not found (`{ "detail": "Note not found" }`)

- **`DELETE /notes/{note_id}`**
  - Response: `{ "deleted": true }`
  - Errors:
    - `404` if note not found (`{ "detail": "Note not found" }`)

### How to run (PowerShell)
From the `backend/` folder (with your venv activated):
- `uvicorn app.main:app --reload --port 8000`

If port `8000` is blocked on your machine, use another port (example):
- `uvicorn app.main:app --reload --port 8010`

### Quick manual test (PowerShell)
Health:
- `Invoke-RestMethod http://localhost:8000/health`

Create note:
- `Invoke-RestMethod -Method Post http://localhost:8000/notes -ContentType "application/json" -Body '{"title":"First note"}'`

List notes:
- `Invoke-RestMethod http://localhost:8000/notes`

Delete note (replace `{id}`):
- `Invoke-RestMethod -Method Delete http://localhost:8000/notes/{id}`

