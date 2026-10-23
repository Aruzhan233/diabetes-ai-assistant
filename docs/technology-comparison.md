# Technology Comparison

We compared alternatives for five key decisions and chose Flutter, FastAPI, PostgreSQL, gradient boosting with a rule-based baseline, and an API-hosted LLM with guardrails. Each choice is driven by the project requirements: a team of two developers, one codebase for the patient app and the doctor panel, relational medical data with strict access control, and explainable AI. Ratings (High / Medium / Low) are given against our requirements, not in general.

### Mobile client and doctor panel

| Option | One codebase for iOS, Android, web | Team experience | Charts and UI libraries | Effort for 2 people | Decision |
| --- | --- | --- | --- | --- | --- |
| **Flutter (Dart)** | High: mobile + Flutter Web | Medium | High (e.g. fl_chart) | Low | **Chosen** |
| React Native | Medium: web needs a separate React app | Medium | High | Medium | Alternative |
| Native (Kotlin + Swift) | Low: two apps + separate web | Low | High | High | Rejected |

Justification: one codebase for the patient/parent app and the doctor web panel halves UI work for a two-person team.

### Backend and REST API

| Option | Fit with Python AI stack | Data validation | Auto API docs | Performance | Decision |
| --- | --- | --- | --- | --- | --- |
| **FastAPI (Python)** | High: same language as pandas, scikit-learn | High (Pydantic) | Yes (OpenAPI) | High (async) | **Chosen** |
| Django + DRF (Python) | High | Medium | Via add-ons | Medium | Alternative |
| Node.js (Express / NestJS) | Low: AI code needs a separate service | Medium | Via add-ons | High | Rejected |

Justification: the AI modules and the API live in one language; Pydantic schemas double as input validation for medical values.

### Data storage

| Option | Relational data (users, roles, consents, readings) | Access control and integrity | Time-series queries | Hosting control | Decision |
| --- | --- | --- | --- | --- | --- |
| **PostgreSQL** | High | High: foreign keys, transactions, row-level security | Medium-High (indexes, window functions) | Self-hosted or managed | **Chosen** |
| MongoDB | Medium | Medium: no joins or FK constraints | Medium | Self-hosted or managed, on servers in Kazakhstan | Alternative |
| Firebase Firestore | Low | Medium: security rules, vendor lock-in | Low | Vendor only | Rejected |

Justification: consents, roles and the audit log are relational and must stay consistent; health data should stay on infrastructure we control.

### Risk scoring (hypo- / hyperglycemia)

| Option | Works on small tabular data | Explainability | Effort | Role | Decision |
| --- | --- | --- | --- | --- | --- |
| **Rule-based thresholds** | High | High | Low | Baseline | **Chosen as baseline** |
| **Gradient boosting (XGBoost / LightGBM)** | High | Medium-High (feature importance, SHAP) | Medium | Improved model | **Chosen** |
| Logistic regression | High | High | Low | Second comparison | Alternative |
| LSTM / GRU | Low: needs frequent CGM data | Low | High | Only if CGM data used | Optional |

Justification: manual glucose entry gives sparse tabular data, where gradient boosting performs well; rules give a transparent baseline to beat.

### AI coach chat

| Option | Implementation speed | Answer quality | Privacy | Cost / hardware | Decision |
| --- | --- | --- | --- | --- | --- |
| **LLM via API + guardrails** | High | High | Medium: no identifiers or raw glucose history are sent | Pay per use | **Chosen** |
| Self-hosted open model (Llama, Qwen) | Low | Medium | High | Needs GPU | Alternative |
| Rule-based FAQ bot | High | Low | High | Free | Fallback |

Justification: the fastest path to a useful coach; no names or identifiers leave the backend, and a safety test sets guards medical boundaries; no personal data or glucose history leave the backend, using an external API is not a cross-border transfer of personal data under Law No. 94-V; a self-hosted model remains the alternative if this changes.

Other choices (not compared in detail): Firebase Cloud Messaging for push alerts, JWT for authentication, Docker and GitHub Actions for CI, PyTorch with EfficientNet/MobileNet for optional food recognition.
