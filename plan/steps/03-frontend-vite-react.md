## Step 3 — Frontend (Vite + React + TypeScript)

### Goal
Build a single-page UI with a left “menu” of note titles and simple create/delete actions.

### UI layout (simple)
- **Left panel**
  - “New note” form (title input + Create button)
  - List of notes (titles). Clicking selects a note.
- **Right panel**
  - Selected note details: title + created_at
  - Delete button (with confirmation)

### Suggested components
- `NewNoteForm`
- `NotesMenu`
- `NoteDetails`

### API integration (frontend)
Base URL: `http://localhost:8000`
- `GET /notes`
- `POST /notes`
- `GET /notes/{id}`
- `DELETE /notes/{id}`

### Behavior checklist
- On load: fetch notes and show them in menu.
- On create: post note, then refresh list (or append).
- On select: fetch note details (or reuse list data if it contains enough).
- On delete: confirm, delete, then refresh list and clear selection if needed.
- Show simple loading/error text (no fancy UI needed).

### Commands
From repo root (or `frontend/`):
- `npm install`
- `npm run dev`


