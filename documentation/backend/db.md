## `db.py` — SQLite storage layer

### Purpose
`db.py` contains the minimal SQLite access functions used by the FastAPI routes. It:
- opens a SQLite connection
- creates the `notes` table (if missing)
- provides small CRUD helper functions

### Database file location
The DB file is created at:
- `backend/notes.db`

This comes from:
- `DB_PATH = Path(__file__).resolve().parent.parent / "notes.db"`

### Schema
Table: `notes`
- `id` (TEXT, primary key)
- `title` (TEXT, not null)
- `created_at` (TEXT, not null, ISO datetime string)

Created by `init_db()`:
- `CREATE TABLE IF NOT EXISTS notes (...)`

### Functions
- **`get_conn() -> sqlite3.Connection`**
  - Opens a connection to `notes.db`
  - Sets `row_factory = sqlite3.Row` so rows can be accessed like dicts (e.g. `row["title"]`)

- **`init_db() -> None`**
  - Creates the `notes` table if it does not exist

- **`list_notes() -> List[Tuple[str, str, str]]`**
  - Returns a list of `(id, title, created_at)`
  - Ordered by `created_at DESC` (newest first)

- **`get_note(note_id: str) -> Optional[Tuple[str, str, str]]`**
  - Returns `(id, title, created_at)` if found, otherwise `None`

- **`create_note(note_id: str, title: str, created_at: str) -> None`**
  - Inserts a new note row
  - Note: `id` must be unique (SQLite will error on duplicate primary key)

- **`delete_note(note_id: str) -> bool`**
  - Deletes a row by id
  - Returns `True` if something was deleted, else `False`

### Notes / limitations (current MVP)
- Only a **title** is stored (no note body/content yet).
- No “update” function yet (editing comes later).

