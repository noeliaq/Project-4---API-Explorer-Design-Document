# Module 4 Project — Part 2: Study Tracker API Design

**Your Name:** Noelia Quiroga
**Date:** October 30th, 2026


---

## The App: Study Tracker

Design a REST API for a "Study Tracker" app. Students can:
- Log study sessions (course, duration, notes)
- Set study goals (target hours per course per week)
- View their progress and summaries

---
## Application description
The Study Tracker API provides a centralized system for students to organize their academic workload. Students can log individual study sessions with durations and notes, define weekly time based on goals per course, and review progress summaries to stay on schedule 


## Section 1 — Resources

List the resources your API will manage. Suggested: Students, Courses, Study Sessions, Goals.

| Resource      | Key Attributes |
|---------------|--------------- |
| students      | student_id, name, email 
| courses       | course_id, course_name, description, credits, instructor_name
| study_sessions| session_id, student_id (foreign key), course_id (foreign key), duration, date, notes
| goals         | goal_id, student_id(foreign key), course_id (foreign key), title, target_hours, status(pending, completed)

---

## Section 2 — Relationships

Describe how your resources relate to each other:

- Student ↔ Course: _(describe)_ : Can one student have more than one course-YES 
                                   A student can enroll in multiple courses          
                                   Can one Course have more than one student-YES 
                                   A course can have many enrolled students
                                   Many to Many (N:N)
- Student ↔ Study Session: _(describe)_ Can one student have more than one session - YES
                                   A student can log in multiple study sessions during the year
                                   Can one study session have more than one student - NO
                                   Each session belong to the student who logged in
                                   One to Many (1:N)
- Course ↔ Study Session: _(describe)_ Can one course have more than one session - YES
                                   A course can accumulate many study session over time
                                   Can one study session have more than one course - NO
                                   A study session is blocked for one specific course
                                   One to Many (1:N)
- Student ↔ Goal: _(describe)_ Can one student have more than one goal - YES
                                   A student can sets weekly goals for each of their courses
                                   Can a goal have more than one student - NO
                                   Each goal belongs to the specific student 
                                   One to Many (1:N)

---

## Section 3 — Endpoints

Design at least:
- Full CRUD for study_sessions (5 endpoints)
- 3+ endpoints for related resources
- 1+ filtering endpoint  

| Method |     URI                           |       Description                | Auth Required? |
|--------|-------------------------------    |  --------------------------------|----------------|
|  GET   |  /study_sessions                  |  List all sessions               |     YES
|  GET   |  /study_sessions?course_name=Math |  Filter sessions by courses      |     YES
|  GET   |  /study_sessions/{id}             |  Get a session details           |     YES
|  POST  |  /study_sessions                  |  Create new a study session      |     NO
|  PUT   |  /study_sessions/{id}             |  Update a session notes          |     YES
|  DELETE|  /study_sessions/{id}             |  Delete a session                |     YES
|  GET   |  /students/{id}/progress          |  Get the progress for a student  |     YES
|  GET   |  /students                        |  List of all students            |     YES  
|  GET   |  /students/{id}                   |  Get a student details           |     YES
|  POST  |  /students                        |  Register new student            |     NO
|  GET   |  /students/{id}/courses            |  List of courses for a student   |     YES
|  GET   |  /courses                         |  List of all courses             |     NO
|  PUT   |  /courses/{id}                    |  Update a course details         |     YES
|  DELETE|  /courses/{id}                    |  Delete specific course          |     YES
|  GET   |  /students/{id}/goals              |  List of all goals for a student |     YES
|  GET   |  /goals                           |  List of all goals               |     YES (Admin)
|  PUT   |  /goals/{id}                      |  Update a goal's details         |     YES
---

## Section 4 — Request/Response Schemas

### POST /study_sessions — Create a new session

**Request body:**
```json
{
    "student_id": 1,				                    ← integer 
	"course_id": 2,				                        ← integer
    "duration": 45, 					                ← integer
    "date": "2026-12-01",		                        ← date (YYY-MM-DD)
    "notes": "Study session for APIs design document"	← string

}
```

**Success response (201):**
```json
{
    "session_id": 36,                                           ← integer
    "student_id": 1,				                    ← integer 
	"course_id": 2,				                        ← integer
    "duration": 45, 					                ← integer
    "date": "2026-12-01",		                        ← date (YYY-MM-DD)
    "notes": "Study session for APIs design document"	← string

}
```

### POST /students — Add a new student

**Request body:**
```json
{
    "name": "Richard Smith",				            ← string 
	"email": "richards@test.com"				        ← string
}
```

**Success response (201):**
```json

{
    "student_id": 1,                                            ← integer
    "name": "Richard Smith",				            ← string 
	"email": "richards@test.com",				        ← string

}
```
### GET /students/{id}/progress

**Response (200):**
```json
{
    "student_id": 1,                                    ← integer
    "period": {
        "period_start": "2026-10-01",                   ← date (YYYY-MM-DD)
        "period_end": "2026-11-01"                      ← date (YYY-MM-DD)
    },
    "course_progress": [
        {       
            "course_id": 1,
            "course_name": "APIs Design Documentation", ← string
            "target_minutes": 120,                      ← integer
            "completed_minutes": 60,                    ← integer
            "pending_minutes": 60,                      ← integer
            "goal_met": false                           ← boolean
        }
    ]
}
```
### GET /courses/{id}
```json
    {
      "course_id": 1,                                    ← integer
      "course_name": "APIs Design Documentation",        ← string
      "instructor": "Mario Lopez",                       ← string
      "credits": 3,                                      ← integer
      "description":"API documentation tells you what endpoints exist, what data to send, and what you'll get back"   ← string
    }

```
---

## Section 5 — Authentication

| Endpoint                    | Auth Required|           Notes                        |
|-----------------------------|--------------|----------------------------------------|
| GET /courses                |    NO        | any student have access to all courses |
| POST /study_sessions        |    NO        | any students can add new sessions      |
| GET /students/{id}/progress |   YES        | only creator can see their progress    |
| DELETE /study_sessions/{id} |   YES        | only creator can delete a session     |

**Auth method and rationale:**
> _Write here._
Method:
    JSON Web Tokens (JWT) transmitted via Authorization: Bearer <token>
 
Stateless Authentication: JWTs store user identity (student_id) directly in the signed token, eliminating unnecessary database lookups on every request.

Role & Resource Authorization: For private routes (GET /students/{id}/progress and DELETE /study_sessions/{id}), the server decodes the token to verify the ownership, returning 403 Forbidden if a student attempts to view or delete another student's data.

---

## Section 6 — Error Responses for POST /study_sessions

| Status Code |       When it occurs              |
|-------------|-----------------------------------|
| 201         | Created - a new session is created|
| 400         | Bad request - when server cannot understand your request (missing a field 'braces')             |
| 401         | Unauthorized - need to authenticate - API key/token - only occurs if authentication is  enabled                                             |
| 404         | Not Found - foreign key missing (student_id and course_id)                                       |
| 422         | Unprocessable Entity - Request format is correct but data doesn't pass validation( -45 minutes) |
