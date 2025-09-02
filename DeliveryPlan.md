# Weekly Delivery Plan (MVP): Core Questionnaire + Company Matching

## Week 1 — Data & migrations

**Do**
- Finalize PostgreSQL schema: `users`, `profiles`, `forms`, `jobs`, `matches`.
- Write migrations and a seed script with realistic jobs.
- Add indexes on key fields (job tech tags, `user_id`, `status`, FKs).
- Provide a simple SQL view/query for draft “top-N matches” (no AI).

**Verifiable outcome**
- Repo boots a clean DB with one command; seeds are loaded.
- README has `migrate/seed` commands.
- Example SQL returns a top jobs list.

---

## Week 2 — Backend API scaffold + Swagger

**Do**
- Express app skeleton, config, error handling, `GET /health`.
- Job read endpoints: `GET /jobs`, `GET /job/:id`.
- Basic profile endpoints: `GET /user/profile`, `PATCH /user/profile` (no auth yet).
- Swagger (OpenAPI) with request/response examples.

**Verifiable outcome**
- All routes respond locally.
- Swagger opens and supports “try it out”.

---

## Week 3 — Form submit & baseline matching

**Do**
- `POST /form/submit`: persist answers into `forms`, upsert `profiles`, set status=`received`.
- `GET /match/jobs`: return top-N based on deterministic scoring (weights from README), no AI yet.
- Store short `reasons` for job cards.

**Verifiable outcome**
- In Postman: submit form → get matches with score and reasons.

---

## Week 4 — AI agent + background worker

**Do**
- Define and document JSON contract (request/response) between backend and AI agent.
- Background worker: polls `forms` with `status='received'`, calls AI agent, writes `profiles.derived_profile` and `matches`, flips form status to `done/error`.
- AI client supports two modes: **mock** (local) and **real** (via env vars).
- Retries/backoff and logging.

**Verifiable outcome**
- A submitted form becomes real `matches` after some time, even without the frontend.
- Logs show successful/failed processing cycles.

---

## Week 5 — Frontend: Questionnaire & processing status

**Do**
- React Native Questionnaire screen: controlled inputs, validation, submit to `POST /form/submit`.
- Processing/Status screen: user sees that data is being processed; periodic readiness check (by `forms.status` or simply re-fetching `GET /match/jobs`).
- Base states: loading / empty / error.

**Verifiable outcome**
- On device/simulator you can fill the form, see “processing”, and wait for results.

---

## Week 6 — Frontend: Matches list & Job details

**Do**
- Matches List screen: `GET /match/jobs`, show score and concise reasons.
- Job Details screen: `GET /job/:id`.
- Mini-analytics (SDK or simple events): `form_submitted`, `matches_viewed`, `job_opened`.

**Verifiable outcome**
- User sees a ranked list and can open job details; analytics events are sent.

---

## Week 7 — Tests, stability, documentation

**Do**
- Integration tests for the pipeline: submit → worker → matches → API output.
- Worker error handling (no crashes, mark `error`, retry policy).
- Finish Swagger examples for all routes.
- Trim N+1 queries, verify indexes, basic metrics/logs.

**Verifiable outcome**
- Tests green; Swagger covers all endpoints; worker is resilient.

---

## Week 8 — Polish & demo

**Do**
- Demo data for multiple scenarios (strong/weak matches).
- UI polish (placeholders, empty states, copy).
- Update README and diagrams (flow, components, sequence) to match implementation.
- Short demo script.

**Verifiable outcome**
- From a clean start you can run the project, complete the user flow, and show a stable result.
