## `models.py` — API request/response models

### Purpose
`models.py` defines the Pydantic models used by the FastAPI routes for:
- validating incoming request bodies
- shaping outgoing responses

### Models
- **`NoteCreate`**
  - Used by: `POST /notes`
  - Fields:
    - `title: str`
      - minimum length: 1
      - maximum length: 200

- **`NoteOut`**
  - Used by: `GET /notes`, `POST /notes`, `GET /notes/{note_id}`
  - Fields:
    - `id: str` (UUID string)
    - `title: str`
    - `created_at: str` (ISO datetime string, generated in UTC)

- **`HealthOut`**
  - Used by: `GET /health`
  - Fields:
    - `status: str` (currently always `"ok"`)

### Notes / limitations (current MVP)
- There is no “update/edit” request model yet (that comes when we add `PUT /notes/{note_id}`).

