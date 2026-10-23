# Requirements Specification

## 1. Functional requirements

Twenty-two functional requirements are defined. Each one is linked to its source: a finding from the survey (F), the patient interview (E2) or the doctor interview (D) in the Evidence of Relevance document, or a decision from the Literature Review (Lit., numbers as in its reference list). Must requirements form the MVP for pre-defense 2; Should and Could requirements are delivered if time allows.

| ID | Requirement | Priority | Source | Verified by |
| --- | --- | --- | --- | --- |
| FR-01 | The user registers and creates a profile with diabetes type (1 or 2), treatment type (insulin or tablets), glucose units (mmol/L or mg/dL) and language | Must | D7 | E2E-1 |
| FR-02 | The user enters a glucose reading manually with a tag (fasting, before meal, after meal, bedtime, night) | Must | F3, D1 | E2E-2 |
| FR-03 | The user logs meals (carbohydrates in grams or bread units), insulin doses, medication, physical activity and symptoms | Must | D1, E2 | Unit and API tests |
| FR-04 | The app classifies each reading by default thresholds (< 3.0, < 3.9, 3.9–10.0, > 10.0, > 13.9 mmol/L) | Must | D5; Lit. [1], [3] | Unit tests, E2E-2 |
| FR-05 | A critical value triggers a push alert to the patient and all linked parents; urgent low alerts are delivered even during quiet hours | Must | F1, F2, F4, D5 | E2E-3, alert delivery tests |
| FR-06 | The user sets personal alert thresholds and quiet hours | Should | F6, D5, D8 | Unit tests |
| FR-07 | A reading below 3.0 mmol/L opens an emergency screen with clear steps (15 g carbohydrate rule) and a call for help | Must | D5; Lit. [1] | E2E-3 |
| FR-08 | The patient invites a parent or doctor with an invite code and can revoke access at any time | Must | F7, D3; Lit. [16], [19] | E2E-1, E2E-4 |
| FR-09 | An adolescent chooses which data types a parent sees (alerts only or full history) | Should | F7; Lit. [16], [17] | Access control tests |
| FR-10 | The parent sees the child’s readings, alerts and the time of the last sync; for younger children the parent can enter data | Must | F2; E1 (5 of 20 children do none of the tasks alone) | E2E-2, E2E-3 |
| FR-11 | The doctor dashboard shows a weekly report: chart, average, min/max, number of hypo- and hyperglycaemia episodes, measurement regularity, HbA1c and medications | Must | D2 | API tests, E2E scenario |
| FR-12 | The doctor dashboard states that the doctor does not monitor the patient in real time | Must | D3 | Manual check |
| FR-13 | The meal bolus calculator suggests a meal dose from carbohydrates and the doctor-set ratio; it is blocked below 3.9 mmol/L, limited by a maximum dose and confirmed by the user | Should | E2; Lit. [11], [20] | Unit tests, E2E-7 |
| FR-14 | At bedtime, the ML model predicts the risk of nocturnal hypoglycaemia (low / medium / high); with too few recent readings it shows "Not enough data" | Must | F1, F4, F5; Lit. [21], [5] | Evaluation 3.1, E2E-6 |
| FR-15 | Every risk warning shows its reason in plain language (top contributing factors) | Must | F8, D6; Lit. [6], [8] | Evaluation 3.1 (SHAP check) |
| FR-16 | Trend analysis shows the share of readings in range, averages by time of day and repeated lows or highs | Should | D4 | Evaluation 3.2 |
| FR-17 | The AI coach answers educational questions using guideline documents (RAG) and refuses dose and diagnosis requests | Should | D6, F8; Lit. [13]–[15] | Evaluation 3.3 |
| FR-18 | The app reminds the user to measure glucose, take medication and log meals | Should | D4, E2 (forgotten bolus) | Unit tests |
| FR-19 | Each reading can be tagged with stress, sport or sleep | Should | E2, D4 | Unit tests |
| FR-20 | The interface is available in Kazakh and Russian | Should | E2, E1 | Usability test |
| FR-21 | The user can export and delete their data | Should | Lit. [18], [19] | API tests |
| FR-22 | Food photo recognition estimates carbohydrates, which the user confirms before use | Could | E2 | Evaluation 3.4 |

The administrator manages users and roles and reviews the audit log (covered by NFR-03 and NFR-05). Reminders for regular check-ups (HbA1c, kidney tests, eye exam) are a possible extension (D8).

## 2. Non-functional requirements

Twelve non-functional requirements are defined, eight of them with Must priority. Each has an acceptance criterion that can be run or inspected before the final pre-defense. Target values marked “proposed” are draft values for supervisor review.

| ID | Category | Requirement | Priority | Acceptance criterion (how we verify) |
| --- | --- | --- | --- | --- |
| NFR-01 | Security | All client-server traffic is encrypted | Must | Every API endpoint is reachable only over HTTPS (TLS 1.2+); a plain HTTP request is refused or redirected |
| NFR-02 | Security | Passwords are stored only as salted hashes | Must | Database inspection shows bcrypt/argon2 hashes, no plain text; unit test on registration |
| NFR-03 | Access control | Each role sees only data it is allowed to see | Must | Automated tests: parent, doctor and patient request another user’s data without consent and receive HTTP 403 in 100% of cases |
| NFR-04 | Privacy / consent | Patient can grant and revoke access at any time | Must | After an invite code is revoked, the doctor or parent loses access on the next request (integration test) |
| NFR-05 | Auditability | Access to health data and sensitive actions are logged | Must | Every read of another user’s data and every consent change creates an audit record (user, action, time); checked by test |
| NFR-06 | Privacy | No real patient data or secrets in the repository | Must | .gitignore and secret scanning in CI; only synthetic or public datasets in the repository |
| NFR-07 | Medical safety | AI never gives insulin doses or diagnoses | Must | 100% pass on the AI safety test set (Section 4); every AI answer shows a disclaimer and refers medical decisions to a doctor |
| NFR-08 | Reliability | Critical-value alerts are delivered quickly | Must | Push alert to the patient and linked parent within 60 s of entering a critical value in 95% of test runs (proposed) |
| NFR-09 | Performance | API responds fast under normal load | Should | 95th percentile response time under 500 ms with 50 simulated concurrent users (proposed) |
| NFR-10 | Usability | Logging a glucose reading is quick | Should | A new user logs a reading in 3 taps or fewer and under 15 s in usability tests with 5 participants |
| NFR-11 | Maintainability | Code is tested and checked automatically | Should | Backend test coverage of 70% or more; tests run in GitHub Actions on every pull request |
| NFR-12 | Portability | App runs on both mobile platforms and the web | Could | Patient app runs on Android 10+ and iOS 15+; doctor panel works in current Chrome and Safari |

The user profile stores diabetes_type (type 1 or type 2); the non-functional requirements apply to both types equally.
