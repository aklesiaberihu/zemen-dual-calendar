# Zemen — Dual Calendar Web Application

A full-stack collaborative scheduling platform for managing events across both the **Ethiopian** and **Gregorian** calendar systems.

Built by Aklesia Berihu Mekonnen and Meron Zemichael Kahsay.

---

## Features

- **Dual-calendar support** — view and manage events in Ethiopian and Gregorian calendars simultaneously
- **Date conversion** — instantly convert any date between the two calendar systems, including the 13th month (Pagume)
- **Event management** — full CRUD with recurrence (daily/weekly), timezone support, and full-text search
- **Shared events** — collaborate with role-based access (owner, editor, viewer)
- **Optimistic locking** — concurrent edits are detected with version conflict resolution (`409 Conflict` + force-save option)
- **Version history & diff** — every event update is snapshotted, with field-level diffs between any two versions
- **Smart scheduling** — interval merging, gap finding, and constraint-based slot ranking across all participants
- **Email reminders** — SMTP-based background daemon sends reminders before events
- **Google OAuth 2.0** — sign in with Google and export events to Google Calendar
- **Holiday management** — Ethiopian and Gregorian public holidays seeded on startup

---

## Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Frontend** | React 19, Vite 7, React Router v7 |
| **Backend** | FastAPI (Python 3.11) |
| **Database** | PostgreSQL 16 (SQLAlchemy ORM) |
| **Auth** | JWT + Google OAuth 2.0 |
| **Email** | SMTP (`smtplib` + TLS) |
| **Deployment** | Docker Compose |
| **Testing** | Pytest (158 unit tests) + Postman (125 integration tests) |

---

## Getting Started

### Prerequisites
- Docker and Docker Compose installed

### Run the Project

1. Clone the repository:
   ```bash
   git clone [https://github.com/aklesiaberihu/zemen-dual-calendar.git](https://github.com/aklesiaberihu/zemen-dual-calendar.git)
   cd zemen-dual-calendar
   ```

2. Copy the environment config and fill in your credentials:
   ```bash
   cp docker-compose.yml docker-compose.local.yml
   # Edit docker-compose.local.yml and replace placeholder values:
   # - GOOGLE_CLIENT_ID
   # - GOOGLE_CLIENT_SECRET
   # - SMTP_PASSWORD
   # - SECRET_KEY
   ```

3. Start the application:
   ```bash
   docker compose -f docker-compose.local.yml up --build
   ```

4. Open in your browser:
   - **Frontend:** `http://localhost:5173`
   - **Backend API Docs (Swagger UI):** `http://localhost:8000/docs`

---

## Testing

### Unit Tests (Pytest)
```bash
cd backend
python3 -m pytest tests/ -v
```
158 tests across 13 test files covering authentication, date conversion edge cases (including Pagume / month 13), interval merging, slot ranking, scheduling pipeline, diffs, reminders, security, input validation, and UTC storage.

### API Integration Tests (Postman)
Import `postman_collection.json` into Postman to run the 125-test integration suite covering performance (NFR-P1), role enforcement (NFR-S1), and endpoint validation. Set `base_url` to `http://localhost:8000` and run the collection top-to-bottom.

---

## Project Structure

```text
zemen-dual-calendar/
├── backend/
│   ├── app/
│   │   ├── core/               # Business logic (conversion, intervals, ranking, diff, auth)
│   │   ├── models/             # SQLAlchemy ORM models
│   │   ├── routers/            # FastAPI route handlers
│   │   ├── schemas/            # Pydantic request/response schemas
│   │   └── services/           # Background services (email reminders)
│   ├── tests/                  # Pytest unit test suite (158 tests)
│   └── main.py
├── frontend/
│   └── src/
│       └── pages/              # React page components
├── UML diagrams/               # Architecture and UML diagrams
├── docs/                       # Project documentation and reports
├── postman_collection.json     # Postman API integration test collection
└── docker-compose.yml
```
