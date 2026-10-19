# Project Proposal

**Project title:** “Development of a Mobile Application for Diabetes Monitoring and Management Using Artificial Intelligence Methods”  
**Тақырыбы:** «Жасанды интеллект әдістерін қолдана отырып, қант диабетін мониторингтеу және басқаруға арналған мобильді қосымшаны әзірлеу»  
**Тема:** «Разработка мобильного приложения для мониторинга и управления сахарным диабетом с использованием методов искусственного интеллекта»  
**Working name:** Diabetes AI Assistant  
**Team:** Aruzhan Toktarbekova, Ainur Boranova (group SE-2411)  
**Supervisor:** Altynbek Seitenov

## 1. Problem Statement

Diabetes management is a daily task. A patient has to measure glucose several times a day and keep track of meals, insulin or medication, physical activity and symptoms. In practice this creates four problems:

- Records are incomplete and scattered. Glucose values stay in the meter, while food, insulin and activity are written on paper or not recorded at all.
- Patients do not understand why glucose changes. A meter shows a number but does not link it to the meal, the insulin dose or the activity that caused it. Estimating carbohydrates in a meal is especially difficult.
- Parents are not informed in time. Parents of children and adolescents with type 1 diabetes need to know immediately about dangerous low or high values, even when the child is not with them.
- Doctors lack a continuous picture. At an appointment the endocrinologist usually sees only a few measurements and has no clear summary of the patient’s patterns between visits.

Existing solutions solve parts of this problem: glucose meters store readings, CGM sensors send their own alerts, and diary apps provide logging and charts. However, they rarely combine in one system (1) unified logging of glucose, food, insulin and activity, (2) explainable AI analysis of trends and risks, (3) consent-based sharing with parents and the doctor, and (4) safe handling of sensitive medical data. This project addresses that gap.

## 2. Target Users

*Table 1 – Target user groups*

| User group | Role in the system |
|---|---|
| Primary: people with type 1 diabetes, especially children and adolescents | Log glucose, meals, insulin, activity and symptoms; view charts and trends; receive alerts; use the AI coach |
| Primary: parents / guardians of minors | Parent mode: follow the child’s data after consent and receive critical-value alerts |
| Secondary: adults with type 2 diabetes | Use the basic features (logging, charts, reports, reminders); the profile stores the diabetes type |
| Doctors (endocrinologists, therapists) | Web dashboard: view the patient’s data and weekly reports only with the patient’s consent (invite code) |
| Platform administrators | Manage users and roles and review the audit log |

The user groups are summarised in Table 1. Type 1 diabetes is the main focus because alerts, parent mode and the insulin log are most valuable for this group, and this is the main difference from ordinary trackers. Type 2 is supported with shared basic features only.

## 3. Aim

To design and develop a mobile application with a backend and AI modules that supports daily diabetes monitoring, early detection of risky situations, and informed communication between the patient, parents/guardians and the doctor — without replacing medical decisions.

## 4. Objectives

1. Analyse the problem through interviews with patients, parents and medical specialists (3–5 discussions) and document the evidence of relevance, separating verified findings from assumptions.
2. Review scientific literature (at least 12 sources, including 8 or more scientific publications) and compare 3–5 existing systems and the key technology alternatives.
3. Specify functional and non-functional requirements with priorities (Must / Should / Could) and verifiable acceptance criteria.
4. Design the architecture (system context, use case and component diagrams, data model) and UX prototypes of the key scenarios.
5. Implement the core platform: authentication and role-based access control; logging of glucose, food, insulin/medication, activity and symptoms; charts and weekly reports; critical-value alerts; parent mode and doctor access by invite code; audit log.
6. Implement the AI modules: trend analysis, hypo-/hyperglycemia risk scoring (rule-based baseline, then a machine-learning model), and an AI coach chat with safety guardrails; food photo recognition with carbohydrate estimation if time allows.
7. Verify the system against the requirements and evaluate the AI modules against a baseline on synthetic and public data.
8. Manage the project in GitHub and YouTrack with traceable individual contributions and regular supervisor review.

## 5. Expected Outcomes

- Working prototype: a Flutter mobile app for patients and parents, a Flutter Web dashboard for doctors, and a FastAPI backend with a PostgreSQL database covering the core scenarios.
- AI modules with measured quality: risk scoring, trend analysis and AI coach, each compared with a simple baseline using defined metrics.
- Evidence of safety and privacy: passing access-control tests, an audit log, and a 100% pass rate on the AI safety test set (no insulin doses, no diagnoses).
- Documentation: evidence of relevance, literature review, system and technology comparisons, requirements, architecture, verification and evaluation results.
- Repository: a GitHub repository with traceable history linked to YouTrack tasks, and tags for each pre-defense submission.

## 6. Project Scope

### 6.1 In scope

- Manual entry of glucose readings and logging of meals, insulin/medication, physical activity and symptoms.
- Charts, time in range, trends and weekly reports for the patient and the doctor.
- Four roles — patient, parent/guardian, doctor, administrator — with consent management (invite code, revoke access).
- Critical-value push alerts to the patient and linked parents; an urgent flag in the doctor dashboard.
- Rule-based and machine-learning risk scoring, trend analysis, and an AI coach chat in recommendation-only mode.
- Audit log of data access and sensitive actions.
- Food photo recognition with carbohydrate estimation (optional, “Could” priority).

### 6.2 Out of scope

- Medical diagnosis and treatment decisions.
- Automatic calculation or recommendation of insulin doses.
- Direct integration with CGM sensors and insulin pumps.
- Gestational diabetes and other specific forms of diabetes; separate type 2 features beyond the basic ones.
- Clinical validation, medical-device certification and commercial deployment.
- Use of real patient data without informed consent.

### 6.3 Constraints and assumptions

- A team of two students and a fixed academic schedule; requirements are prioritised so that the Must features are delivered first.
- Medical safety: the AI provides explanations and general guidance only, shows a disclaimer and always refers medical decisions to a doctor.
- Health data are sensitive: security and privacy requirements apply from the first sprint.
- AI modules are trained and evaluated only on synthetic data (simglucose) and public datasets (OhioT1DM if access is granted; Food-101 / Nutrition5k for food).

## 7. Selected Technologies

*Table 2 – Selected technology stack*

| Layer | Technology |
|---|---|
| Mobile app (patient, parent) | Flutter (Dart) |
| Doctor dashboard | Flutter Web (shared code with the mobile app) |
| Backend and REST API | Python, FastAPI, Pydantic, SQLAlchemy, Alembic |
| Authentication and access | JWT (access and refresh tokens), role-based access, invite codes |
| Database | PostgreSQL |
| Push notifications | Firebase Cloud Messaging |
| Risk scoring and analysis | pandas, NumPy, scikit-learn, XGBoost / LightGBM; baseline: threshold rules |
| AI coach | LLM via API with anonymised context, guardrails and a safety test set |
| Food recognition (optional) | PyTorch, pretrained EfficientNet / MobileNet fine-tuned on Food-101 |
| Testing and DevOps | pytest, flutter_test, Docker, Docker Compose, GitHub Actions |
| Design and management | Figma, PlantUML / draw.io, GitHub, YouTrack |

The selected technologies are listed in Table 2. Detailed comparisons of alternatives for each key decision are given in the Technology Comparison document.

## 8. Engineering Complexity

The project is not a simple CRUD application. It requires the integration of several independent parts into one secure system:

- Multi-client architecture: a mobile client and a web dashboard working with one backend and one data model.
- Security and privacy: role-based access control, consent management with invite codes and revocation, an audit log, encrypted traffic and hashed passwords.
- Real-time alerts: critical values must reach the patient and linked parents within a defined time.
- Several AI components of different types: statistical trend analysis, a machine-learning classifier for risk, an LLM-based chat with safety guardrails and, optionally, a computer-vision model.
- Measurable evaluation: a verification plan with acceptance criteria, defined metrics (recall, precision, F1, false alerts per day, safety pass rate) and a baseline for every AI module.

## 9. Planned Milestones

*Table 3 – Project milestones*

| Milestone | Content |
|---|---|
| Pre-defense 1 (24 Oct 2026) | Proposal, evidence of relevance, literature review, system and technology comparisons, requirements, diagrams, UX prototype, verification plan; GitHub and YouTrack set up |
| Pre-defense 2 | Core implementation: authentication, roles and consent, logging, charts, alerts, audit log; first tests in CI |
| Final pre-defense | AI modules, full testing and evaluation against baselines, final documentation |
| Final defense | Presentation and demonstration of the completed system |

## 10. Main Risks and Mitigation

*Table 4 – Main risks and mitigation measures*

| Risk | Mitigation |
|---|---|
| Medical harm from incorrect AI advice | Recommendation-only mode, disclaimers, no dose calculation, safety test set with a 100% pass requirement |
| Leak of sensitive health data | Role-based access, consent management, audit log, HTTPS, only anonymised summaries sent to the LLM, no real data or secrets in the repository |
| Low accuracy of AI features | Rule-based baseline, confidence levels, manual correction by the user, honest reporting of limitations |
| No access to real or frequent glucose data | Synthetic data from simglucose; OhioT1DM requested under a data-use agreement; LSTM/GRU forecasting only if frequent data are available |
| Time constraints | Must / Should / Could prioritisation; food recognition is optional |
| Regulatory classification | Positioned as an informational support tool, not a medical device; consultation with specialists |
