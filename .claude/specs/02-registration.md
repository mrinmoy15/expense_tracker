# Spec: Registration

## Overview
Implement user registration so visitors can create a Spendly account. The `/register`
route currently only handles GET and renders the form. This step wires up the POST
handler: it validates the submitted fields, checks email uniqueness, hashes the
password, inserts the new user, and redirects to `/login` on success. It also adds
`app.secret_key` to `app.py`, which is required for Flask sessions used in later steps.

## Depends on
- Step 01 — Database Setup (`database/db.py` with `get_db()`, `init_db()`, `seed_db()`)

## Routes
- `GET  /register` — render the registration form — public
- `POST /register` — process form submission, create user, redirect to `/login` — public

## Database changes
No new tables or columns. Uses the existing `users` table:
- `name` TEXT NOT NULL
- `email` TEXT UNIQUE NOT NULL
- `password_hash` TEXT NOT NULL
- `created_at` TEXT DEFAULT (datetime('now'))

## Templates
- **Modify:** `templates/register.html`
  - Form already has `method="POST" action="/register"` and `{% if error %}` block — no structural changes needed
  - Verify the `name` field has `name="name"`, `email` has `name="email"`, `password` has `name="password"` (already correct)

## Files to change
- `app.py`
  - Add `app.secret_key` (required for sessions in later steps)
  - Add `request, redirect, url_for` to the Flask import
  - Convert `register()` from GET-only to a GET+POST handler

## Files to create
None.

## New dependencies
No new dependencies.

## Rules for implementation
- No SQLAlchemy or ORMs — use raw `sqlite3` via `get_db()`
- Parameterised queries only — never format values into SQL strings
- Hash passwords with `werkzeug.security.generate_password_hash`
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- Close the DB connection in a `finally` block or immediately after use
- Validation order: name present → email present → password min 8 chars → email unique
- On any validation failure: re-render `register.html` with the `error` variable set
- On success: `redirect(url_for('login'))`
- Do NOT log the user in automatically after registration — that is Step 3

## Definition of done
- [ ] `GET /register` renders the form without errors
- [ ] Submitting the form with valid data inserts a new row in `users` and redirects to `/login`
- [ ] Password is stored as a hash — never as plain text — confirmed by inspecting the DB
- [ ] Submitting with an empty name shows an inline error on the form
- [ ] Submitting with an empty email shows an inline error on the form
- [ ] Submitting with a password shorter than 8 characters shows an inline error
- [ ] Submitting with an email that already exists shows an inline error (no duplicate row inserted)
- [ ] Successful registration does NOT create a session — `/profile` still returns its stub string
- [ ] App starts without errors (`uv run python app.py`)
