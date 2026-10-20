# Diabetes AI Assistant

**Development of a Mobile Application for Diabetes Monitoring and Management Using Artificial Intelligence Methods**

Жасанды интеллект әдістерін қолдана отырып, қант диабетін мониторингтеу және басқаруға арналған мобильді қосымшаны әзірлеу

Разработка мобильного приложения для мониторинга и управления диабетом с использованием методов искусственного интеллекта

> ⚠️ **Research prototype.** This application is a diploma research project. It is not a medical device and has not been clinically validated. It does not provide insulin dosages, diagnoses or treatment decisions. In an emergency, contact a doctor or emergency services.

---

## Project status

**Current stage:** proposal and design (milestone `pre-defense-1`, 24 Oct 2026).

Requirements, literature review, technology comparison and system diagrams are in progress. Implementation of the mobile app and backend has not started yet.

---

## About the project

Diabetes AI Assistant is a mobile application for people with **type 1 diabetes**, their parents and their doctors. The patient records glucose readings, the system analyses trends, identifies risky situations and sends alerts to the patient and, with consent, to a parent or doctor.

### Development stages

| Stage | Data source | Risk assessment |
|---|---|---|
| **1. MVP** | Manual glucose readings entered by the patient | Rule-based thresholds and trend analysis |
| **2. If time allows** | Integration with continuous glucose monitoring (CGM) | Machine learning prediction of hypoglycemia |

**Why manual readings first.** A glucose meter gives 4–7 readings per day. This is enough for logging, trends and threshold alerts, but too sparse for reliable short-term prediction. CGM gives a reading every 5 minutes, which makes ML prediction feasible. So the MVP uses rule-based logic, and the ML model is developed on CGM research data in parallel.

### MVP features

- Registration and roles: patient, parent, doctor, administrator
- Manual entry of glucose readings
- History and trends: average glucose, time in range, change between readings
- Rule-based alerts with three levels of response:
  - **Information** — e.g. weekly average glucose
  - **Recommendation** — e.g. "glucose is low, re-check in 15 minutes"
  - **Medical attention** — e.g. "critically low glucose, contact your doctor or emergency services"
- Parent and doctor access, granted and revoked by the patient
- Web panel for doctors
- AI assistant for general questions, with safety restrictions

### Future work

- CGM integration
- ML-based hypoglycemia prediction inside the app
- Food recognition from photos

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
              │   Backend   │───────► Notification service
              └──┬───────┬──┘
                 │       │
          ┌──────▼──┐ ┌──▼─────────┐
          │Database │ │ AI module  │
          └─────────┘ └────────────┘
```

The backend technology will be selected based on the technology comparison (DIP-12).

Diagrams are stored in [`diagrams/`](diagrams):

- System Context Diagram
- Component Diagram
- Use Case Diagram
- ERD, Activity and Sequence diagrams (in progress)

---

## AI methodology (research component)

| | |
|---|---|
| **Task** | Prediction of hypoglycemia (< 3.9 mmol/L) 30–60 minutes ahead |
| **Dataset** | CITY (CGM Intervention in Teens and Young Adults with T1D), JAEB Center for Health Research |
| **Input features** | Recent glucose values, rate and direction of change, time of day, insulin and carbohydrates where available |
| **Models** | Rule-based baseline → Logistic Regression → XGBoost / LightGBM |
| **Explainability** | SHAP values: which factors contributed to each prediction |
| **Data split** | By patient (no patient appears in both train and test) |
| **Metrics** | Precision, recall, F1, false alarms per day, missed events, prediction lead time |

The model is evaluated offline on the CITY dataset. Good results on a research dataset do not make the model clinically reliable.

### Glucose thresholds

Based on international consensus on time in range:

| Level | Range |
|---|---|
| Level 2 hypoglycemia | < 3.0 mmol/L |
| Level 1 hypoglycemia | < 3.9 mmol/L |
| Target range | 3.9–10.0 mmol/L |
| Level 1 hyperglycemia | > 10.0 mmol/L |
| Level 2 hyperglycemia | > 13.9 mmol/L |

Thresholds and alert scenarios are planned to be reviewed with an endocrinologist.

---

## Medical safety and data protection

- **AI assistant restrictions:** no insulin dosages, no diagnoses, no advice to stop or change treatment. Requests on these topics get a fixed safe response.
- **Data quality:** physiologically impossible values are rejected; readings older than a set limit are marked as outdated.
- **Offline alerts:** rule-based alerts and local notifications work without internet; critical alerts repeat until acknowledged.
- **Access control:** role-based access; the patient grants and revokes access for parents and doctors; access is logged.
- **Minors:** parental consent is stored as a consent record; a parent account is linked to the patient account.
- **Data protection:** encrypted transport (HTTPS) and storage of sensitive data.

---

## Team

| | Aruzhan Toktarbek | Ainur Boranova |
|---|---|---|
| **Implementation** | Backend: API, database, access control, notifications | Mobile application (Flutter): patient and parent screens, glucose entry, alerts |
| **Design** | Functional requirements, Use Case diagram, UX mockups of key scenarios | Non-functional requirements, System Context and Component diagrams, technology comparison |
| **Research** | Literature review part 1, existing systems comparison (criteria, systems 1–2), user interviews set 1 | Literature review part 2 and synthesis, existing systems comparison (systems 3–5), user interviews set 2, verification and evaluation plan |
| **Project management** | Repository and README, project proposal and scope | YouTrack setup, milestones, risks and dependencies |

Tasks are tracked in YouTrack (project `DIP`). Every task has one assignee, and commits reference the task ID, e.g. `DIP-1: Add initial repository structure and README`.

---

## Repository structure

```
├── config/      # Configuration files
├── diagrams/    # System Context, Component, Use Case, ERD, Activity, Sequence
├── docs/        # Proposal, project management plan, literature review
├── src/         # Source code (mobile app, backend)
├── tests/       # Tests
└── README.md
```

The CITY dataset is not included in this repository because of its data use terms.

---

## Timeline

| Date | Milestone |
|---|---|
| 22 Oct 2026 | Meeting with academic supervisor |
| 24 Oct 2026 | First pre-defense (`pre-defense-1`) |

**Academic supervisor:** Altynbek Seitenov
