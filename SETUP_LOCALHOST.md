# CyberTrace (NISHAD) — Localhost Setup (Windows)

This is a **single FastAPI server** — it serves the frontend HTML/CSS/JS *and*
the backend API from the same process, on the same port. You do **not** need
two separate servers/ports; you only run one.

`backend/.env` is already pre-filled to use **SQLite** (a local file,
`cybertrace.db`) so nothing extra needs installing — no MySQL, no Postgres,
no cloud database. If you'd rather use MySQL, edit `DATABASE_URL` in
`backend/.env` (a MySQL example is left commented above it).

## 1. Open a terminal in the project

```
cd NISHAD\backend
```

## 2. Create and activate a virtual environment

```
python -m venv venv
venv\Scripts\activate
```

## 3. Install backend dependencies

```
pip install -r requirements.txt
```

## 4. Seed the database (creates tables, the admin account, and the 3 cases)

```
python seed.py
```

You should see `[seed] Admin account created — username: "admin"`. The
password is whatever `ADMIN_PASSWORD` is set to in `backend/.env`
(pre-filled as `admin123` — change it if you like, then re-run `seed.py`
is safe to re-run any time).

## 5. Start the server

```
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

## 6. Open the site

```
http://127.0.0.1:8000
```

That single URL serves everything: the login page, `cases.html`,
`competition.html`, `admin.html`, `leaderboard.html`, and every `/api/...`
call the frontend makes — all same-origin, so there is no CORS/"Failed to
fetch" issue like the deployed version had.

- **Admin login:** username `admin`, password from `backend/.env`
  (`admin123` by default).
- **Team logins:** log in as admin → "Create Team + Leader Login" to create
  a team and its leader account, then log in as that leader/member.

To stop the server: `Ctrl+C` in the terminal. To restart later, just repeat
step 2 (activate) and step 5 (run) — no need to re-seed unless you deleted
`cybertrace.db`.
