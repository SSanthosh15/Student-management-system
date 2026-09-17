# API Documentation — Student Management System

Base URL: `http://127.0.0.1:5000/api`

All request/response bodies are JSON. All endpoints return
appropriate HTTP status codes and a JSON error body on failure.

---

## GET /api/health

Health check.

**Response 200**
```json
{ "status": "ok" }
```

---

## GET /api/students

Returns all students. Supports optional search filtering.

**Query params**
- `search` (optional) — matches against `name` or `course`

**Example**
```
GET /api/students
GET /api/students?search=cse
```

**Response 200**
```json
[
  { "id": 1, "name": "Priya Sharma", "email": "priya@example.com", "course": "B.Tech CSE", "marks": 88.5 },
  { "id": 2, "name": "Arjun Rao", "email": "arjun@example.com", "course": "B.Tech ECE", "marks": 76 }
]
```

---

## GET /api/students/{id}

Returns a single student by ID.

**Response 200**
```json
{ "id": 1, "name": "Priya Sharma", "email": "priya@example.com", "course": "B.Tech CSE", "marks": 88.5 }
```

**Response 404**
```json
{ "error": "Student with id 99 not found." }
```

---

## POST /api/students

Creates a new student.

**Request body**
```json
{
  "name": "Priya Sharma",
  "email": "priya@example.com",
  "course": "B.Tech CSE",
  "marks": 88.5
}
```

**Response 201** — returns the created record including its new `id`.

**Response 400** (validation failure)
```json
{
  "error": "Validation failed.",
  "details": ["Field 'email' must be a valid email address."]
}
```

**Response 409** (duplicate email)
```json
{ "error": "A student with this email already exists." }
```

---

## PUT /api/students/{id}

Fully updates an existing student. All fields required.

**Request body:** same shape as POST.

**Response 200** — returns the updated record.
**Response 404 / 400 / 409** — same semantics as POST.

---

## PATCH /api/students/{id}

Partially updates an existing student — send only the fields to change.

**Request body example**
```json
{ "marks": 91 }
```

**Response 200** — returns the updated record.

---

## DELETE /api/students/{id}

Deletes a student record.

**Response 200**
```json
{ "message": "Student with id 1 deleted successfully." }
```

**Response 404**
```json
{ "error": "Student with id 99 not found." }
```

---

## Error Response Shape

All error responses follow this shape:

```json
{ "error": "<human-readable message>", "details": ["optional", "list", "of", "specifics"] }
```

| Status | Meaning |
| ------ | ------- |
| 400 | Validation failed / malformed JSON |
| 404 | Resource not found |
| 405 | HTTP method not allowed on that route |
| 409 | Conflict (duplicate unique field) |
| 500 | Unexpected server/database error |
