## Goal
Build a **very simple Notes app** with a **React + TypeScript + Vite** frontend and a **Python FastAPI** backend (served via **uvicorn**).

The app should only:
- **Create notes**
- **Show a menu/list of notes by title**
- **Delete notes**

## MVP Scope (what “done” means)
- **Create**: user can add a note with a title.
- **List**: user sees a menu/list of note titles and can select one.
- **Delete**: user can delete a note.
- **Persistence**: notes survive restart (SQLite).
- **Basic UX**: loading + empty state + delete confirmation.

## Data model
- **Note**
  - `id`: string (UUID)
  - `title`: string
  - `created_at`: ISO datetime

## Backend (FastAPI)
### Tech choices
- FastAPI + uvicorn
- SQLite storage (simple table `notes`)
- Pydantic models with **type hints** everywhere
- CORS enabled for the Vite dev server

### API endpoints
- `GET /health` → `{ "status": "ok" }`
- `GET /notes` → list notes
- `POST /notes` → create note
- `GET /notes/{id}` → get one note (for selection/details)
- `DELETE /notes/{id}` → delete note

### Backend tasks
- Create DB schema + minimal data access layer (SQL + `sqlite3` or SQLAlchemy).
- Implement endpoints + validation + consistent error responses (404 for missing note, 422 for validation, etc.).
- Add CORS middleware (dev-only config allowed).
- Provide `README` instructions + `requirements.txt`.

## Frontend (React + TS + Vite)
### Pages / routes
- `/` Single-page app:
  - left: **menu/list** of note titles + “New note”
  - right: selected note details + “Delete”

### Components
- `NotesList` (menu of titles)
- `NewNoteForm` (title input + create)
- `NoteDetails` (show selected note title + created date)
- `ConfirmDialog` (for delete)
- `Toast`/inline alert (error/success feedback)

### Frontend tasks
- Set up API client module (typed request/response models).
- Implement routing (React Router) and basic layout.
- Handle loading, empty states, and errors cleanly.

## Dev setup & scripts
- Backend:
  - `uvicorn app.main:app --reload --port 8000`
- Frontend:
  - `npm install`
  - `npm run dev` (port 5173)
- Ensure CORS allows `http://localhost:5173` → `http://localhost:8000`.

## Project structure (suggested)
- `backend/`
  - `app/main.py`, `app/models.py`, `app/db.py`
- `frontend/`
  - `src/` components, pages, `api/`

## Nice-to-haves (after MVP)
- Add note body/content
- Edit/update notes
- Search/filter notes