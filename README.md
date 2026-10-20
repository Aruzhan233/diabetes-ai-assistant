# Diabetes AI Assistant

**Development of a Mobile Application for Diabetes Monitoring and Management Using Artificial Intelligence Methods**

Жасанды интеллект әдістерін қолдана отырып, қант диабетін мониторингтеу және басқаруға арналған мобильді қосымшаны әзірлеу

Разработка мобильного приложения для мониторинга и управления диабетом с использованием методов искусственного интеллекта

> ⚠️ **Research prototype.** This application is a diploma research project. It is not a medical device and has not been clinically validated. It does not provide insulin dosages, diagnoses or treatment decisions. In an emergency, contact a doctor or emergency services.

---

## Project status

**Current stage:** proposal and design (milestone `pre-defense-1`, 24 Oct 2026).

Requirements, literature review, existing systems comparison, verification plan and system diagrams are in progress. Implementation of the mobile app and backend has not started yet.

---

## About the project

Diabetes AI Assistant is a mobile application for people with **type 1 diabetes**, their parents and their doctors. The patient enters glucose meter readings, food, insulin, activity and symptoms. The app shows trends, warns about risky situations and sends critical alerts to the patient and, with consent, to a parent. A doctor sees charts and a weekly summary only through an invite code from the patient.

### Development stages

| Stage | Data source | Risk assessment |
|---|---|---|
| **1. MVP** | Manual glucose meter readings (4–7 per day) | Threshold alerts + ML prediction of nocturnal hypoglycemia from sparse readings |
| **2. If time allows** | Integration with continuous glucose monitoring (CGM) | The same model on dense CGM data |

### MVP features

- Registration and roles: patient, parent, doctor, administrator
- Manual entry of glucose readings, food, insulin, activity and symptoms
- Charts and patterns: share of readings in range, averages by time of day, repeated lows or highs
- Alerts with three levels of response:
  - **Information** — e.g. weekly average glucose
  - **Recommendation** — e.g. "glucose is low, re-check in 15 minutes"
  - **Needs attention now** — e.g. "critically low glucose, follow your hypo plan and contact your doctor or emergency services"
- Night-time hypoglycemia risk at bedtime, with an explanation of the main factors
- Parent access and doctor access by invite code, revocable at any time, with an audit log
- Doctor web panel with a weekly summary
- AI coach for general questions, with safety restrictions

### Future work

- CGM integration
- Food recognition from photos

---

## Technology stack

| Part | Technologies |
|---|---|
| Mobile app | Flutter (Android, iOS) |
| Backend | Python, REST API |
| Database | PostgreSQL |
| Push notifications | Firebase Cloud Messaging (FCM) + local notifications on the device |
| AI / ML | Python, scikit-learn, XGBoost / LightGBM, SHAP |
| AI coach | LLM with system prompt, input/output filters and answers grounded in guideline documents |
| Testing | pytest, flutter_test, Locust or k6 |
| Infrastructure | Docker, GitHub Actions (CI, secret scanning) |
| Project management | YouTrack (project `DIP`), GitHub |

---

## Architecture

```
┌──────────────────┐      ┌──────────────────┐
│  Mobile app      │      │  Doctor web panel│
│  (Flutter)       │      │                  │
└────────┬─────────┘      └────────┬─────────┘
         │        REST API         │
         └───────────┬─────────────┘
              ┌──────▼──────┐
              │   Backend   │───────► Notification service (FCM)
              │  (Python)   │
              └──┬───────┬──┘
                 │       │
        ┌────────▼──┐ ┌──▼─────────┐
        │PostgreSQL │ │ AI module  │
        └───────────┘ └────────────┘
```

Diagrams are stored in [`diagrams/`](diagrams).

---

## AI methodology (research component)

**Research question:** can a gradient-boosting model that uses only sparse self-monitoring data (4–7 meter readings per day, carbohydrates, insulin, activity) predict nocturnal hypoglycemia better than a simple rule, without more false alarms? How much accuracy is lost compared with dense CGM data?

| | |
|---|---|
| **Task** | At bedtime, predict nocturnal hypoglycemia (< 3.9 mmol/L for ≥ 15 min) in the next 8 hours |
| **Dataset** | CITY (CGM Intervention in Teens and Young Adults with T1D), Jaeb Center for Health Research: 153 participants aged 14–24 with T1D, 26 weeks, Dexcom G5 CGM |
| **Sparse data** | Labels come from the dense CGM series; model inputs use only simulated meter readings (before meals, at bedtime) with ISO 15197 measurement error |
| **Compared methods** | B0 rule baseline → B1 logistic regression → M1 XGBoost / LightGBM → R1 the same model on dense CGM data (upper reference) |
| **Explainability** | SHAP: the main factors behind each prediction |
| **Data split** | By patient (no participant appears in both train and test) |
| **Main metrics** | Recall, false alarms per patient-night, precision, AUPRC, Brier score, with 95% confidence intervals |

Full plan: [`docs/verification-plan.md`](docs/verification-plan.md).

### Glucose thresholds

Based on ADA Standards of Care in Diabetes—2026:

| Level | Range |
|---|---|
| Level 2 hypoglycemia | < 3.0 mmol/L |
| Level 1 hypoglycemia | < 3.9 mmol/L |
| Target range | 3.9–10.0 mmol/L |
| Level 1 hyperglycemia | > 10.0 mmol/L |
| Level 2 hyperglycemia | > 13.9 mmol/L |

The 3.9 and 3.0 mmol/L warning levels were confirmed by a general practitioner in our first interview. Thresholds and alert wording will be reviewed further with doctors.

---

## Medical safety and data protection

- **AI coach restrictions:** no insulin dosages, no diagnoses, no advice to stop or change treatment. Requests on these topics are refused and redirected to the care team.
- **Data quality:** values outside 1.1–33.3 mmol/L are not saved silently; probable unit mix-ups are checked; with too few or outdated readings, risk is shown as "Not enough data", never as low risk.
- **Offline alerts:** critical alerts are generated on the device and shown immediately, even without internet.
- **Access control:** role-based access; the patient grants and revokes parent and doctor access; every access change is logged.
- **Minors:** parental consent is stored as a consent record.
- **Data protection:** HTTPS only, hashed passwords, no secrets or real patient data in the repository.

---

## Team

| | Aruzhan Toktarbek | Ainur Boranova |
|---|---|---|
| **Implementation** | Backend: API, database, access control, notifications | Mobile application (Flutter): patient and parent screens, glucose entry, alerts |
| **Design** | Functional requirements, Use Case diagram, UX mockups of key scenarios | Non-functional requirements, System Context and Component diagrams, technology comparison |
| **Research** | Literature review part 1, existing systems comparison (criteria, systems 1–2), user interviews set 1 | Literature review part 2 and synthesis, existing systems comparison (systems 3–7), user interviews set 2, verification and evaluation plan |
| **Project management** | Repository and README, project proposal and scope | YouTrack setup, milestones, risks and dependencies |

Tasks are tracked in YouTrack (project `DIP`). Every task has one assignee, and commits reference the task ID, e.g. `DIP-11: Add existing systems comparison`.

---

## Repository structure

```
├── config/      # Configuration files
├── diagrams/    # System Context, Component, Use Case and other diagrams
├── docs/        # Project documents
│   ├── existing-systems.md
│   ├── project-management.md
│   ├── project-proposal.md
│   └── verification-plan.md
├── src/         # Source code (mobile app, backend, ML)
├── tests/       # Tests
└── README.md
```

The CITY dataset is not stored in this repository. Only download instructions and processing scripts will be kept here.

---

## Timeline

| Date | Milestone |
|---|---|
| 12 Oct 2026 | Draft shown to the academic supervisor |
| 22 Oct 2026 | Meeting with the academic supervisor |
| 24 Oct 2026 | First pre-defense before the committee (`pre-defense-1`) |

**Academic supervisor:** Altynbek Seitenov
