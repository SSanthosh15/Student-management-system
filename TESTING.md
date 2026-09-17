# Testing Procedure — Student Management System

Per SOP Section 10 (Testing Procedure). Backend must be running at
`http://127.0.0.1:5000` before running these tests.

## 1. Create — valid data

```bash
curl -X POST http://127.0.0.1:5000/api/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Priya Sharma","email":"priya@example.com","course":"B.Tech CSE","marks":88.5}'
```
**Expected:** `201 Created`, returns the new record with an `id`.

## 2. Create — missing required field

```bash
curl -X POST http://127.0.0.1:5000/api/students \
  -H "Content-Type: application/json" \
  -d '{"name":"","email":"priya@example.com","course":"CSE","marks":88}'
```
**Expected:** `400 Bad Request`, `details` lists the missing/invalid field(s).

## 3. Create — duplicate email

Run test #1 twice with the same email.
**Expected:** second call returns `409 Conflict`.

## 4. Create — invalid data (bad email / out-of-range marks)

```bash
curl -X POST http://127.0.0.1:5000/api/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Test","email":"not-an-email","course":"CSE","marks":150}'
```
**Expected:** `400 Bad Request`, two validation messages (bad email, marks out of range).

## 5. Read — empty database

Delete all records, then:
```bash
curl http://127.0.0.1:5000/api/students
```
**Expected:** `200 OK`, `[]`. Frontend shows "No students found."

## 6. Read — populated database

```bash
curl http://127.0.0.1:5000/api/students
```
**Expected:** `200 OK`, array of student objects.

## 7. Update — valid ID

```bash
curl -X PUT http://127.0.0.1:5000/api/students/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Priya Sharma","email":"priya@example.com","course":"B.Tech CSE","marks":95}'
```
**Expected:** `200 OK`, `marks` updated to 95.

## 8. Update — invalid ID

```bash
curl -X PUT http://127.0.0.1:5000/api/students/999 \
  -H "Content-Type: application/json" \
  -d '{"name":"X","email":"x@example.com","course":"X","marks":50}'
```
**Expected:** `404 Not Found`.

## 9. Delete — valid ID

```bash
curl -X DELETE http://127.0.0.1:5000/api/students/1
```
**Expected:** `200 OK`, confirmation message. Record no longer appears in GET /api/students.

## 10. Delete — invalid ID

```bash
curl -X DELETE http://127.0.0.1:5000/api/students/999
```
**Expected:** `404 Not Found`.

## 11. Verify database values after every operation

After each Create/Update/Delete above, re-run `GET /api/students`
(or inspect `students.db` with a SQLite browser) to confirm the
change was actually persisted, not just returned in the response.

## 12. Frontend responsiveness

- Open `frontend/index.html` at desktop width (≥1024px) — table and
  form should sit in a single readable column with comfortable spacing.
- Resize to mobile width (≤480px) — form fields stack, buttons go
  full-width, and the table scrolls horizontally inside its container
  instead of breaking the page layout.

## 13. Error handling when backend is unavailable

- Stop the Flask server.
- Reload `frontend/index.html`.
**Expected:** the UI shows a red alert banner: "Could not load
students. Is the backend running?" instead of a silent failure or a
raw JS error in the console.

## Test Summary Table (fill in during actual test run)

| # | Test Case | Expected | Actual | Pass/Fail |
| - | --------- | -------- | ------ | --------- |
| 1 | Create valid | 201 | | |
| 2 | Create missing field | 400 | | |
| 3 | Create duplicate email | 409 | | |
| 4 | Create invalid data | 400 | | |
| 5 | Read empty DB | 200, [] | | |
| 6 | Read populated DB | 200, [...] | | |
| 7 | Update valid ID | 200 | | |
| 8 | Update invalid ID | 404 | | |
| 9 | Delete valid ID | 200 | | |
| 10 | Delete invalid ID | 404 | | |
| 11 | DB persistence check | matches | | |
| 12 | Responsive layout | pass | | |
| 13 | Backend-down handling | graceful alert | | |
