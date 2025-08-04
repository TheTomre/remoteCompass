# Remote Compass

Remote Compass is a personalized career tool designed for developers aiming to land their first or second international remote job, but who feel uncertain about the process, their skills, or even where to begin.  
It helps users understand their profile, improve market readiness, and target jobs with clarity and confidence.

## Tech stack

- **Frontend**: React Native, enables cross-platform mobile app development with a large ecosystem and strong community support.
- **Backend**: Node.js with Express, simple, popular framework with great middleware ecosystem for fast API development.
- **Database**: PostgreSQL, robust relational database providing SQL experience and industry relevance.
- **API documentation**: Swagger, standard, developer-friendly tool for documenting and testing API endpoints.

## API Endpoints

### User authentication
- POST /auth/google, authenticate user via Google OAuth

### User profile
- POST /form/submit, submit core questionnaire form
- GET /user/profile, get user profile
- PATCH /user/profile, update user profile

### Job matching and listings
- GET /match/jobs, get list of matched jobs
- GET /jobs, get all jobs (optional fallback or debug endpoint)
- GET /job/:id, get job details

### CV generation
- POST /cv/generate, generate general CV based on user profile

### Application tracking
- POST /tracker/update, update application tracker (record user application actions)


## Database schema (basic outline)
Tables:
- **users**
- **profiles**
- **companies**
- **applications**

## Key integrations

- **OpenAI API**: for generating cover letters and optimizing CV content
- **OAuth authentication**: using Google Sign-In for user accounts
- **Analytics**: Google Analytics or Mixpanel to track user interactions and measure feature usage

## Feature prioritization

| Feature Name               | Priority | Why this priority?                                                       | Include in MVP? |
|----------------------------|----------|--------------------------------------------------------------------------|-----------------|
| Core questionnaire         | High     | Essential starting point to collect user data and preferences            | Yes             |
| Company matching           | High     | Core functionality, presenting relevant companies based on user profile | Yes             |
| Cover letter generation    | Medium   | Adds value but not essential to core flow                                | No              |
| CV optimization            | High     | Key differentiator, personalized, optimized CV improves success chances | Yes             |
| Salary estimation          | Medium   | Useful enhancement, not essential for MVP                                | No              |
| Mock interview simulation  | Low      | Complex to build, not required for MVP                                   | No              |

## Success metrics

| Feature Name          | Success Metric                                                         | What it measures                                    |
|-----------------------|------------------------------------------------------------------------|----------------------------------------------------|
| Core questionnaire    | 80% of users complete the questionnaire after signing up              | Ease of use and onboarding engagement              |
| Company matching      | 60% of users view at least one recommended company in the results     | Relevance and perceived usefulness of match list   |
| CV optimization       | 50% of users generate an optimized CV after viewing a matched company | Value of optimization feature for real applications|

---

## [Potentional future features]

- Add API integration with real job boards (for dynamic vacancy loading)
- Implement mock interview simulation with AI
- Add progress dashboard for user growth tracking

Conceptual flowchart of user journey: <img width="1024" height="1536" alt="Conceptual flowchart of user journey" src="https://github.com/user-attachments/assets/051ada10-835b-4183-a612-773af86e1b22" />

UML: <img width="1536" height="1024" alt="UML" src="https://github.com/user-attachments/assets/9d31fa64-7d18-4fae-b0b9-2666a62d1275" />

Sequence diagram: <img width="1024" height="1536" alt="Sequence diagram" src="https://github.com/user-attachments/assets/3a3f54b8-3a48-4ecd-896b-9e9b226cf352" />


