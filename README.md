# Student Management System

A simple full-stack CRUD web application built per the SOP for
"Complete CRUD-Based Web Application Development."

## 1. Project Overview

A minimal Student Management System that lets a user Create, Read,
Update, and Delete student records (name, email, course, marks)
through a browser UI backed by a REST API and a SQLite database.

## 2. Problem Statement

Institutions need a simple way to record and manage student
information (contact details, enrolled course, and marks) without
relying on spreadsheets or paper records.

## 3. Objectives

- Provide a clean UI to add, view, edit, and delete student records
- Expose a REST API for all CRUD operations
- Persist data reliably in a relational database
- Validate input on both client and server
- Handle errors gracefully with clear messages

## 4. Technology Stack

| Layer    | Technology            |
| -------- | ---------------------- |
| Frontend | HTML, CSS, JavaScript (vanilla, `fetch` API) |
| Backend  | Python, Flask, Flask-CORS |
| Database | SQLite |
| API Testing | Postman / curl |
| Version Control | Git |

## 5. System Architecture

```
Browser (HTML/CSS/JS)
        |
        |  fetch() -> JSON over HTTP
        v
Flask REST API (backend/app.py)
        |
        |  sqlite3
        v
SQLite Database (students.db)
```

## 6. Database Design

Single table: `students`

| Column | Type | Constraints |
| ------ | ---- | ----------- |
| id | INTEGER | PRIMARY KEY, AUTOINCREMENT |
| name | TEXT | NOT NULL |
| email | TEXT | NOT NULL, UNIQUE |
| course | TEXT | NOT NULL |
| marks | REAL | NOT NULL, CHECK (0–100) |

## 7. Folder Structure

```
sms/
├── backend/
│   ├── app.py            # Flask REST API + SQLite logic
│   └── requirements.txt  # Python dependencies
├── frontend/
│   ├── index.html        # UI markup
│   ├── style.css         # Styling (responsive)
│   └── script.js         # Fetch calls, validation, rendering
├── README.md
├── API_DOCUMENTATION.md
└── TESTING.md
```

## 8. Installation & Execution

### Step 1 — Start the backend

```bash
cd backend
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

The API will start at `http://127.0.0.1:5000`. The SQLite database
file (`students.db`) is created automatically on first run.

### Step 2 — Open the frontend

Simply open `frontend/index.html` in a browser (double-click it, or
serve it locally):

```bash
cd frontend
python -m http.server 8000
# then visit http://127.0.0.1:8000
```

CORS is enabled on the backend, so the frontend can call the API
even though they run on different ports/origins.

## 9. CRUD Implementation Details

| Operation | Frontend action | HTTP Method | Endpoint |
| --------- | ---------------- | ----------- | -------- |
| Create | Fill form, click "Add Student" | POST | `/api/students` |
| Read (all) | Page load / search box | GET | `/api/students` |
| Read (one) | (used internally) | GET | `/api/students/<id>` |
| Update | Click "Edit", modify, "Save Changes" | PUT | `/api/students/<id>` |
| Delete | Click "Delete", confirm | DELETE | `/api/students/<id>` |

## 10. Validation Rules

- `name`: required, non-empty, max 100 characters
- `email`: required, must match a valid email pattern, must be unique
- `course`: required, non-empty
- `marks`: required, numeric, between 0 and 100

Both the browser form and the Flask API enforce these rules
independently (client-side for UX, server-side as the source of truth).

## 11. Security Notes

- No secrets or credentials are hard-coded in source.
- All SQL uses parameterized queries (no string-concatenated SQL),
  preventing SQL injection.
- Input is validated and sanitized before reaching the database.
- Frontend escapes rendered text to avoid XSS via injected records.

## 12. Challenges & Solutions

| Challenge | Solution |
| --------- | -------- |
| Frontend and backend run on different origins | Enabled CORS via `Flask-Cors` |
| Duplicate emails could corrupt records | Added a `UNIQUE` DB constraint + friendly 409 error |
| Partial updates vs full updates | Supported both `PUT` (full) and `PATCH` (partial) |

## 13. Future Enhancements

- Add authentication (login) and per-user record ownership
- Add pagination for large student lists
- Add sorting by column
- Switch to PostgreSQL/MySQL for production deployment
- Add automated test suite (pytest) for the API

## 14. Git Repository

Initialize and push this project to your own repository, e.g.:

```bash
git init
git add .
git commit -m "Initial commit: Student Management System CRUD app"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

Remember to add a `.gitignore` excluding `venv/`, `__pycache__/`,
and `students.db`.
