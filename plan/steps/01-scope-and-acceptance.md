## Step 1 — Scope & acceptance criteria (keep it simple)

### What we are building (MVP)
A notes app with:
- **Create** a note (title only)
- **List** notes by title (a “menu”)
- **Select** a note from the menu to view it
- **Delete** a note
- **Persist** notes using SQLite

### What we are NOT building (yet)
- Editing notes
- Searching/filtering
- Note body/content
- Auth/users

### Acceptance checks
- Creating a note adds it to the menu immediately.
- Refreshing the page keeps the notes (data is persisted).
- Deleting a note removes it from the menu immediately.
- Selecting a note shows its details (at least title + created time).


