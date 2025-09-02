# Remote Compass

Remote Compass is a personalized career tool for developers who want to land their first or second international remote job and feel uncertain about the process, their skills, or where to begin.  
The MVP focuses on two core outcomes: build a structured candidate profile from a questionnaire and deliver a ranked list of matching jobs with clear reasons.

## Tech stack

- **Frontend**: React Native  
- **Backend**: Node.js + Express  
- **Database**: PostgreSQL  
- **API docs**: Swagger (OpenAPI)  
- **Analytics**: Google Analytics or Mixpanel SDK

## Architecture

Mobile App ↔ **Backend API** ↔ **PostgreSQL**  
**Backend API** ↔ **AI Agent** (profile derivation and scoring).  
The app never calls the AI provider directly. All AI requests go through the backend and return normalized JSON.

## Data model

Tables for v0.1:

- **users**  
  `id, email, name, created_at`

- **profiles**  
  `user_id (FK users.id), english_level, role_preference, stack_json, work_style, location_pref, dealbreakers_json, updated_at`

- **forms**  
  `id, user_id (FK), answers_json, status ENUM('received','analyzing','done'), created_at`

- **jobs**  
  `id, company, title, stack_json, location, remote BOOLEAN, description, min_seniority, tags_json`

- **matches**  
  `id, user_id (FK), job_id (FK), score_numeric, reasons_json, created_at`

_JSON columns use fixed keys so they are safe to query and index later._

## API endpoints

Base path: `/`

### Questionnaire and profile
- `POST /form/submit` — submit core questionnaire. Saves to `forms`, updates or creates `profiles`, sets status to `received`. Returns `{ "status": "received" }`.
- `GET /user/profile` — get user profile.
- `PATCH /user/profile` — update user profile.

### Matching and jobs
- `GET /match/jobs` — return top N matches for the current user.  
  Example item:
  ~~~json
  {
    "job_id": 123,
    "company": "Acme",
    "title": "Frontend Developer",
    "score": 86,
    "reasons": ["Stack: React+TS match", "Work style: async OK"]
  }
  ~~~
- `GET /jobs` — list all jobs (debug or fallback).
- `GET /job/:id` — job details.

### Auth (phase 2)
- `POST /auth/google` — Google OAuth sign-in.

### Health
- `GET /health` — liveness check.

_All endpoints are described in Swagger with request and response examples._

## AI contract (backend ↔ AI agent)

**Request**
~~~json
{
  "english_level": "upper-intermediate",
  "role_preference": "frontend",
  "stack": {"typescript": true, "react": true, "node": true, "sql": true},
  "work_style": {"async_ok": true, "meetings_per_day_max": 2},
  "location_pref": "EU timezones",
  "dealbreakers": {"no_legacy_over_10_percent": true}
}
~~~

**Response**
~~~json
{
  "derived_profile": {
    "target_roles": ["Frontend Developer", "Fullstack TypeScript"],
    "keywords": ["TypeScript", "React", "Node.js", "PostgreSQL"],
    "seniority": "mid"
  },
  "scoring_weights": {
    "stack": 0.45,
    "role": 0.20,
    "work_style": 0.15,
    "location": 0.10,
    "dealbreakers": 0.10
  },
  "improvement_hints": [
    "Strengthen SQL joins and indexing basics",
    "Prepare examples of data-heavy features"
  ]
}
~~~

_The backend persists `derived_profile` into `profiles` and uses `scoring_weights` in the matching function._

## Matching logic

For each job:
~~~text
score = 100 * (
  0.45 * stack_overlap +
  0.20 * role_match +
  0.15 * work_style_fit +
  0.10 * location_fit +
  0.10 * dealbreakers_fit
)
~~~
- `stack_overlap` ∈ [0..1] — fraction of overlapping technologies from `keywords`.  
- The rest are booleans in v0.1.  
- Store short explanations in `reasons_json` to show on job cards.

## Feature prioritization

| Feature Name       | Priority | Why this priority?                                                     | Include in MVP? |
|--------------------|----------|------------------------------------------------------------------------|-----------------|
| Core questionnaire | High     | Foundation for building the candidate profile                          | Yes             |
| Company matching   | High     | Core value, delivers a ranked list with reasons                        | Yes             |
| CV optimization    | Medium   | Useful differentiator, but not essential for v0.1                      | No              |
| Cover letter gen   | Low      | Nice to have, can follow CV optimization                               | No              |
| Salary estimation  | Medium   | Helpful context, not critical for first release                        | No              |
| Mock interview     | Low      | Complex to build and not needed to prove core value                    | No              |

## Success metrics

| Feature            | Metric                                                                    | Measures                                      |
|--------------------|---------------------------------------------------------------------------|-----------------------------------------------|
| Core questionnaire | 80% of signed-in users complete the questionnaire                         | Onboarding clarity and ease                   |
| Matching           | 60% of users who receive a high-match job (>80) open at least one job     | Relevance of matching and explanations        |
| Matches engagement | Median of 2+ job opens per user on first visit to matches                 | Usefulness of the ranked list                 |

## Diagrams

Conceptual flowchart of user journey:  
https://github.com/user-attachments/assets/051ada10-835b-4183-a612-773af86e1b22

UML (components):  
https://github.com/user-attachments/assets/d424760b-4713-47d6-9599-4f6b16bc7256

Sequence diagram:  
https://github.com/user-attachments/assets/f37c3e0e-34cb-45c2-89d3-63aba806f615

## Future work

- CV generation and job-specific optimization  
- Cover letter generator  
- External job board integrations  
- Mock interview simulation  
- Progress dashboard
