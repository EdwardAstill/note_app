## Step 4 — Wire frontend ↔ backend (dev workflow)

### Goal
Run both servers locally and confirm the UI can create/list/select/delete notes through the API.

### Ports
- Backend: `http://localhost:8000`
- Frontend (Vite): `http://localhost:5173`

### CORS
Backend must allow:
- Origin: `http://localhost:5173`
- Methods: `GET, POST, DELETE`
- Headers: `Content-Type`

### Manual integration test
- Start backend.
- Start frontend.
- In the browser:
  - Create 2 notes with different titles.
  - Refresh page → notes still there.
  - Click each title → details show on right.
  - Delete one → removed from menu immediately.

### Minimal “definition of done”
You can do all actions above without opening the backend docs or the DB.


