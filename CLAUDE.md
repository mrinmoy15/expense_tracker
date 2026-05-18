# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spendly is a lightweight personal expense tracker built with Flask and SQLite.

## Environment

This project uses `uv` for package and environment management.

Always use:
- `uv run python app.py` to run the dev server (port 5001, debug mode)
- `uv add package-name` instead of `pip install`
- `uv run pytest` to run tests
- Never use bare `python`, `pip`, or activate the venv manually.

## Architecture
spendly/
├── app.py                  # All routes — single file, no blueprints
├── database/
│   └── db.py               # SQLite helpers: get_db(), init_db(), seed_db()
├── templates/
│   ├── base.html           # Shared layout — all templates must extend this
│   └── *.html              # One template per page
├── static/
│   ├── css/
│   │   ├── style.css       # Global styles
│   │ 
│   └── js/
│       └── main.js         # Vanilla JS only
└── requirements.txt

Single-file Flask app (`app.py`) with Jinja2 templates, a single CSS file, and SQLite (via `database/db.py`).

**Request flow:** `app.py` route → `render_template()` → Jinja2 template extending `templates/base.html` → `static/css/style.css`

**Route status:**
- Implemented: `/`, `/register`, `/login`, `/terms`, `/privacy`
- Stubbed (return strings, students implement): `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`

**Database** (`database/db.py`): Not yet written. The file lists three functions students must implement — `get_db()` (SQLite connection with row_factory + foreign keys), `init_db()` (CREATE TABLE IF NOT EXISTS), `seed_db()` (sample data).

## Templates

All pages extend `templates/base.html`, which provides the navbar, footer, and `{% block scripts %}` for page-specific JS. The base template loads `static/js/main.js` globally; page JS goes in `{% block scripts %}` in the individual template.

Auth pages (`register.html`, `login.html`) support an optional `{{ error }}` template variable for displaying form errors.

## CSS Design System

All styles live in `static/css/style.css`. Use the CSS custom properties defined in `:root` — never hardcode colours or fonts:

- `--ink` / `--ink-soft` / `--ink-muted` / `--ink-faint` — text hierarchy
- `--paper` / `--paper-warm` / `--paper-card` — backgrounds
- `--accent` (#1a472a green), `--accent-2` (#c17f24 amber), `--danger` — semantic colours
- `--font-display` (DM Serif Display), `--font-body` (DM Sans)
- `--radius-sm` / `--radius-md` / `--radius-lg` — border radii

The hero title accent colour `#18a07a` (teal) is used inline and in `.hero-title-accent` — it intentionally differs from `--accent`.

## Tests

pytest + pytest-flask are installed. Test path is `tests/` (configured in `pyproject.toml`). No tests exist yet; students write them as features are implemented.

Run a single test file: `uv run pytest tests/test_foo.py`
