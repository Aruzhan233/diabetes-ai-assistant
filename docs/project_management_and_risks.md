# Project Management and Risks

This section shows how the work is organised and traced: the Git repository, the YouTrack project, who is responsible for what, the milestones, the dependencies on other people and services, and the main risks with their mitigation. It extends Tables 3 and 4 of the Project Proposal; the same milestones and risks are used.

## 1. Repository and task tracking

**GitHub repository:** [Aruzhan233/diabetes-ai-assistant](https://github.com/Aruzhan233/diabetes-ai-assistant)

**YouTrack project (key DIP):** [diabetes-ai-diploma.youtrack.cloud](https://diabetes-ai-diploma.youtrack.cloud)  

*Table 1 – Repository structure*

| Folder | Content |
| --- | --- |
| docs/ | Proposal, evidence of relevance, literature review, comparisons, requirements, verification plan, this section |
| diagrams/ | PlantUML sources (`*.puml`) and PNG/SVG exports of Figures 1–5 |
| prototype/ | Link to the clickable prototype and PNG exports of the 8 screens |
| mobile/ | Flutter app for patient and parent, and the doctor web panel (Flutter Web) |
| backend/ | FastAPI service: API layer, core services, AI modules; Alembic migrations |
| ml/ | Offline training pipeline: data download scripts (no data), sparse sampling, training, evaluation notebooks |
| tests/ | pytest and flutter_test suites, AI safety prompt set |
| .github/workflows/ | CI: tests, coverage report, secret scanning on every pull request |
| root | README.md, .gitignore, .env.example with placeholder values, docker-compose.yml |

Rules of work, followed from the start of the project:

- **Branches:** main is the reviewed baseline and is protected; work is done in task branches named after the YouTrack task, for example DIP-23-database-schema.
- **Commits:** each commit is one coherent change and starts with the task ID, for example “DIP-23: Add initial database schema and ER diagram”. Each student commits from her own GitHub account.
- **Pull requests:** every change reaches main through a pull request that references the YouTrack task, explains the change and how it was checked; the other team member reviews it before merging.
- **YouTrack tasks:** each task has an assignee, an expected outcome, acceptance criteria, a deadline and a priority (Must / Should / Could, as in the requirements). Its status goes Open → In progress → Review → Done, and Done is set only when the acceptance criteria are met and the pull request link is added to the task.
- **Tags:** the exact version submitted for each stage is tagged pre-defense-1, pre-defense-2 and final-pre-defense; tags are never moved.
- **Confidential data:** no passwords, API keys, real patient data or restricted datasets are committed (NFR-06); secret scanning runs in CI.

## 2. Allocation of responsibilities

Each student owns one part of the system and presents it at the defence. The split is visible in YouTrack (assignee of each task), in GitHub (commits and pull requests in her own modules) and in the documents.

*Table 2 – Responsibilities of the team members*

|   | Aruzhan Toktarbekova | Ainur Boranova |
| --- | --- | --- |
| Area | Product and platform | AI and data |
| Components | Flutter mobile app and doctor web panel; FastAPI: authentication, roles, invite codes, consent, audit log, health log, alert engine, reports; notifications (FCM and local); deployment | Risk model service: rule baseline and ML model, SHAP reasons; trend analysis; AI coach with guardrails and RAG; optional food recognition; offline training pipeline |
| Technologies | Dart, Flutter, FastAPI, PostgreSQL, SQLAlchemy, JWT, FCM, Docker | Python, pandas, scikit-learn, XGBoost / LightGBM, SHAP, simglucose, LLM API |
| Requirements | FR-01–FR-13, FR-18–FR-21; NFR-01–NFR-06, NFR-08–NFR-12 | FR-14–FR-17, FR-22; NFR-07; evaluation of the AI modules |
| Documents | Existing systems and technology comparison, requirements, architecture and diagrams, UX prototype, data protection, verification of the platform | Literature review on prediction, XAI and AI safety; research question; datasets; AI evaluation plan; medical safety and the AI safety test set |
| Expected result | Working core: logging, charts, sharing, alerts; passing tests for NFR-01–05 and NFR-08 | Risk model with measured quality against the baseline; AI safety test set with 100% pass (NFR-07) |

Done together: interviews and the survey, the API contract between the backend and the risk model service, assembly of the documents and slides, and review of each other’s pull requests and sections.

## 3. Milestones and plan

*Table 3 – Milestones*

| Milestone | Date | Deliverables | Tag |
| --- | --- | --- | --- |
| Pre-defense 1 | 24 Oct 2026 | Proposal, evidence of relevance, literature review, system and technology comparisons, requirements, architecture and diagrams, UX prototype, verification plan, this section; repository and YouTrack set up | pre-defense-1 |
| Pre-defense 2 | [date] | Core platform (all Must FR of the platform): authentication, roles and consent, logging, charts, alerts, parent mode, doctor access, audit log; tests in CI; risk model baselines B0 and B1 on CITY | pre-defense-2 |
| Final pre-defense | [date] | AI modules integrated (risk model M1, trends, AI coach); full verification and evaluation against baselines; usability and load tests; final documentation | final-pre-defense |
| Final defense | [date] | Presentation and demonstration of the completed system | — |

Proposed plan from pre-defense 1 to pre-defense 2, in two-week sprints (to be adjusted when the date of pre-defense 2 is known):

*Table 4 – Sprint plan to pre-defense 2 (proposed)*

| Sprint | Aruzhan (platform) | Ainur (AI and data) |
| --- | --- | --- |
| Sprint 1 | Project skeleton, Docker Compose, CI; database schema and migrations; registration, login, JWT and roles (FR-01, NFR-02, NFR-03) | CITY data access and preprocessing; sparse meter sampling with ISO 15197 error; labels for nocturnal hypoglycaemia |
| Sprint 2 | Health log and validation (FR-02, FR-03, FR-19); alert engine and thresholds (FR-04–FR-07); mobile logging screens | Feature builder; rule baseline B0 and logistic regression B1 with patient-level cross-validation; draft API contract for the risk model service |
| Sprint 3 | Invite codes, consent and revoke, audit log (FR-08–FR-10, NFR-04, NFR-05); FCM and local notifications (NFR-08); doctor weekly report (FR-11, FR-12) | First XGBoost model M1 and SHAP check; AI safety prompt set (≥ 50 prompts) reviewed by the supervisor |
| Before PD2 | E2E-1 to E2E-4, access control and alert delivery tests in CI; tag pre-defense-2 | Baseline results report; risk model stub behind the agreed API for integration |

## 4. Dependencies

*Table 5 – Dependencies*

| Dependency | Needed for | Status | If it is late or unavailable |
| --- | --- | --- | --- |
| CITY dataset (Jaeb Center), data-use terms | Training and evaluation of the risk model (FR-14) | Requested | Start on simglucose synthetic data; report the limitation |
| Endocrinologist interview and review | Thresholds, bolus calculator (FR-13), alert wording | Planned | Review by the general practitioner and the supervisor; bolus calculator stays “Should” |
| General practitioner feedback | Mockups, sample weekly report, SHAP plausibility check | Agreed | Supervisor review only; stated as a limitation |
| LLM provider API (access and cost) | AI coach (FR-17) | Not started | Self-hosted open model or rule-based FAQ bot (fallback in the technology comparison) |
| Firebase Cloud Messaging | Push alerts to parents (FR-05, NFR-08) | Not started | Local notifications on the patient’s phone always work; parent sees alerts on the next sync |
| Server in Kazakhstan for deployment | Hosting of health data | Not started | Demonstration on a local Docker deployment with synthetic data only |
| Internal: API contract backend ↔ risk model service | Integration of the AI modules | Draft in Sprint 2 | Backend uses a stub that returns “Not enough data” |
| Internal: database schema → health log → alert engine → reports | Order of platform work | Planned in Sprints 1–3 | Tasks are ordered in YouTrack with “depends on” links |

## 5. Risks

Probability (P) and impact (I) are rated High / Medium / Low. Each risk has an owner who watches its early sign and starts the mitigation. The risk list is reviewed at every supervisor meeting.

*Table 6 – Risk register*

| ID | Risk | P | I | Mitigation | Owner |
| --- | --- | --- | --- | --- | --- |
| R1 | Medical harm from incorrect AI advice | M | H | Recommendation-only AI; no doses or diagnoses; bolus calculator is a deterministic formula with doctor-set ratio, low-glucose block and maximum dose; AI safety test set with 100% pass | Ainur |
| R2 | Leak of sensitive health data | L | H | Role-based access, consent and revoke, audit log, HTTPS, hashed passwords; no identifiers or glucose history sent to the LLM; data on servers in Kazakhstan; no real data in the repository | Aruzhan |
| R3 | The ML model does not beat the rule baseline | M | M | Fixed evaluation protocol and baselines; the result is reported honestly – it is also a valid finding; the app keeps the rule-based alerts | Ainur |
| R4 | No or late access to CITY data | M | H | Request sent early; synthetic simglucose data as fallback; evaluation code prepared in advance | Ainur |
| R5 | Not enough time for all features | H | M | Must / Should / Could priorities; Must features first; food recognition is optional; scope reviewed at each milestone | Both |
| R6 | Critical alerts are not delivered (no internet, push failure) | M | H | Local alert on the phone without internet; retries and delivery status; “Parent not yet notified” shown; delivery tests (NFR-08) | Aruzhan |
| R7 | Integration problems between the backend and the AI modules | M | M | API contract agreed in Sprint 2; stub service; integration tests in CI | Both |
| R8 | One team member is unavailable for some time | L | M | Code review of every pull request and shared documents, so each student knows the other’s part | Both |
| R9 | Regulatory classification as a medical device | L | M | Positioned as an informational research prototype; disclaimer in the app; no clinical use without certification | Both |
| R10 | Doctors and parents do not trust or use the app | M | M | Reasons shown for every warning; mockups reviewed by the doctor; usability test with 5 participants; Kazakh and Russian interface | Aruzhan |

## 6. Next steps after pre-defense 1

- **Both:** record the committee’s comments as YouTrack tasks with assignees and deadlines.
- **Both:** set the date of pre-defense 2 in Table 3 and adjust the sprint plan.
- **Aruzhan:** repository skeleton, CI and database schema (Sprint 1).
- **Ainur:** confirm CITY access and prepare the sparse sampling script (Sprint 1).
- **Both:** arrange the endocrinologist interview and send the mockups and a sample report to the general practitioner.
