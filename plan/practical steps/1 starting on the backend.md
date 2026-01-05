uv init backend
cd backend
uv add fastapi uvicorn pydantic

mkdir app
cd app

create these files in app/:
db.py - this contains the database connection and queries
```python
import sqlite3
from pathlib import Path
from typing import List, Optional, Tuple

DB_PATH = Path(__file__).resolve().parent.parent / "notes.db"


def get_conn() -> sqlite3.Connection:
    conn = sqlite3.connect(DB_PATH)
    conn.row_factory = sqlite3.Row
    return conn


def init_db() -> None:
    with get_conn() as conn:
        conn.execute(
            """
            CREATE TABLE IF NOT EXISTS notes (
              id TEXT PRIMARY KEY,
              title TEXT NOT NULL,
              created_at TEXT NOT NULL
            )
            """
        )
        conn.commit()


def list_notes() -> List[Tuple[str, str, str]]:
    with get_conn() as conn:
        rows = conn.execute(
            "SELECT id, title, created_at FROM notes ORDER BY created_at DESC"
        ).fetchall()
        return [(row["id"], row["title"], row["created_at"]) for row in rows]


def get_note(note_id: str) -> Optional[Tuple[str, str, str]]:
    with get_conn() as conn:
        row = conn.execute(
            "SELECT id, title, created_at FROM notes WHERE id = ?",
            (note_id,),
        ).fetchone()
        if row is None:
            return None
        return (row["id"], row["title"], row["created_at"])


def create_note(note_id: str, title: str, created_at: str) -> None:
    with get_conn() as conn:
        conn.execute(
            "INSERT INTO notes (id, title, created_at) VALUES (?, ?, ?)",
            (note_id, title, created_at),
        )
        conn.commit()


def delete_note(note_id: str) -> bool:
    with get_conn() as conn:
        cur = conn.execute("DELETE FROM notes WHERE id = ?", (note_id,))
        conn.commit()
        return cur.rowcount > 0
```

models.py
```python
from pydantic import BaseModel, Field


class NoteCreate(BaseModel):
    title: str = Field(min_length=1, max_length=200)


class NoteOut(BaseModel):
    id: str
    title: str
    created_at: str


class HealthOut(BaseModel):
    status: str
```

main.py
```python
from datetime import datetime, timezone
from typing import List
from uuid import uuid4

from fastapi import FastAPI, HTTPException
from fastapi.middleware.cors import CORSMiddleware

from app.db import create_note, delete_note, get_note, init_db, list_notes
from app.models import HealthOut, NoteCreate, NoteOut

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_credentials=False,
    allow_methods=["GET", "POST", "DELETE"],
    allow_headers=["Content-Type"],
)


@app.on_event("startup")
def on_startup() -> None:
    init_db()


@app.get("/health", response_model=HealthOut)
def health() -> HealthOut:
    return HealthOut(status="ok")


@app.get("/notes", response_model=List[NoteOut])
def notes_list() -> List[NoteOut]:
    rows = list_notes()
    return [NoteOut(id=n_id, title=title, created_at=created_at) for (n_id, title, created_at) in rows]


@app.post("/notes", response_model=NoteOut)
def notes_create(payload: NoteCreate) -> NoteOut:
    note_id = str(uuid4())
    created_at = datetime.now(timezone.utc).isoformat()
    create_note(note_id=note_id, title=payload.title, created_at=created_at)
    return NoteOut(id=note_id, title=payload.title, created_at=created_at)


@app.get("/notes/{note_id}", response_model=NoteOut)
def notes_get(note_id: str) -> NoteOut:
    row = get_note(note_id)
    if row is None:
        raise HTTPException(status_code=404, detail="Note not found")
    (n_id, title, created_at) = row
    return NoteOut(id=n_id, title=title, created_at=created_at)


@app.delete("/notes/{note_id}")
def notes_delete(note_id: str) -> dict:
    deleted = delete_note(note_id)
    if deleted is False:
        raise HTTPException(status_code=404, detail="Note not found")
    return {"deleted": True}
```

Test the backend:
cd backend
uv sync
uv run uvicorn app.main:app --reload --port 8010

This starts the backend server on port 8010, the front end will be able to make requests to this server once it is set up.


To test the backend use this command (in a new terminal because the backend is running on the one you just set up):

Invoke-RestMethod http://127.0.0.1:8010/health

The Invoke-RestMethod cmdlet is a PowerShell command that allows you to make HTTP requests to a URL.