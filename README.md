# Campus — Student Portal

A responsive, Apple-inspired college dashboard with separate HTML, CSS and JavaScript files. No build step or dependencies required. Fonts load from Google Fonts when online; system fonts work offline.

## Run

Extract the ZIP, keep index.html, style.css and script.js together, and open index.html in a modern browser. VS Code Live Server can also serve the folder. Data persists in the current browser's localStorage; persistence for file:// URLs depends on the browser. Use Live Server for consistent behavior.

## Included

- Student and faculty demo views with responsive navigation.
- Overview with calculated attendance and average marks.
- Subject attendance register, faculty editing and CSV export.
- Results table, grading, manual marks editing and validated CSV marks upload.
- Searchable notices, category filters, faculty posting and removal.
- Weekly sample timetable, dark mode, keyboard accessible dialogs.
- Changes stored in your browser with storage failure feedback.

Switch “View as” to Faculty to edit the sample student’s records. Changes appear in Student view. This is a one-student frontend demonstration, not a complete college backend. All records, college branding, schedule and notices are sample data.

## Marks CSV upload

Use a UTF-8 .csv file, maximum 100 KB. Exact header:

```csv
subject_code,marks
CS201,87
CS202,82
CS203,76
MA201,92
HS201,85
```

Partial updates are allowed. Unknown or duplicate subject codes, non-integer marks, scores outside 0–100 and malformed rows reject the whole upload before changing marks. The downloadable results report has extra columns and is not an import template.

## Java / Spring Boot / MySQL / Thymeleaf integration

The supplied files implement the frontend only. They do not include a Java server, database connection, real login, or secure role authorization. The demo role dropdown is not a security boundary.

For a Spring Boot project:

1. Put index.html under src/main/resources/templates/portal.html.
2. Put style.css under src/main/resources/static/css/style.css.
3. Put script.js under src/main/resources/static/js/script.js.
4. Change the stylesheet URL to /css/style.css and script URL to /js/script.js in portal.html. A Thymeleaf application may use th:href="@{/css/style.css}" and th:src="@{/js/script.js}".
5. Add a controller returning the portal view. Replace the seeded localStorage store in script.js with your server data or fetch calls.
6. Implement authenticated sessions and STUDENT/FACULTY authorization with Spring Security; derive the user and role from the server. Validate student ownership and faculty subject assignments on every endpoint. Protect mutations with CSRF tokens and hash passwords securely.
7. Store users, courses, enrollments, attendance, marks and notices through Spring Data JPA repositories in MySQL. Configure credentials outside the repository.

Suggested API contract (not implemented):

| Method | Path | Purpose |
| --- | --- | --- |
| GET | /api/me | Authenticated profile and role |
| GET | /api/me/attendance | Student attendance |
| GET | /api/me/marks | Published student results |
| GET | /api/notices | Notices visible to current user |
| GET | /api/timetable | Current user's timetable |
| PUT | /api/faculty/students/{id}/attendance/{courseId} | Authorized attendance update |
| PUT | /api/faculty/students/{id}/marks/{courseId} | Authorized marks update |
| POST | /api/faculty/marks/import | Validated bulk marks upload |
| POST | /api/faculty/notices | Publish notice |
| DELETE | /api/faculty/notices/{id} | Remove authorized notice |

Demo grading and the 75% attendance threshold are illustrative; replace them with your institution's rules. Do not store actual student records or passwords in this local demonstration.
