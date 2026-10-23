# UX / Prototype

The prototype shows how patients, parents and doctors will use the system in the key scenarios. It is a clickable, high-fidelity mockup with eight screens: seven screens of the mobile app (patient and parent) and the doctor web panel. The screens use the same data across the story of one patient, Aliya, 14, with type 1 diabetes, so the scenarios can be followed from start to end. Buttons and links work: a value can be entered and classified, the critical alert can be acknowledged, the coach answers the suggested questions and an invite code can be created or access revoked.

**Clickable prototype:** https://claude.ai/artifact/AUx1JR9D9qqHJNZmxKYogT

**Screens in the repository:** prototype/ folder (PNG exports of every screen).

All names, readings and dates on the screens are invented demo data. The prototype is in English for review; the final interface will be in Kazakh and Russian (FR-20).

## 1. Design principles

The principles come from the evidence of relevance and the requirements, not from general taste.

- **Fast logging:** a reading is saved with three taps: value, “When?” tag, “Save reading” (NFR-10; E2: the patient stopped keeping a paper diary because it took too long).
- **Three alert levels:** every message has one of three levels – “Information”, “Recommendation”, “Needs attention now” – as agreed for the medical expert review; colour is always paired with a text label.
- **A reason for every warning:** every risk warning shows its main reasons in plain language (FR-15; F8: parents trust AI only “if it is not wrong”).
- **No doses, no diagnoses:** no screen suggests an insulin dose; the coach and the alert screen refer to the doctor’s plan (NFR-07; D6).
- **Simple and readable:** large glucose values, short text, touch targets of at least 44 px and contrast of at least 4.5:1 (D8: “simple interface, large font”).
- **Visible consent:** the patient always sees who can see the data and what they see (F7, D3).

## 2. Key user scenarios

*Table 1 – Key scenarios shown in the prototype*

| # | Scenario | Screens | Requirements | Test |
| --- | --- | --- | --- | --- |
| S1 | Patient logs a reading before dinner and sees that it is in range | Home → Add reading → result card | FR-02, FR-04, FR-19, NFR-10 | E2E-2 |
| S2 | Patient enters 2.8 mmol/L at bedtime; the alert opens, the parent is notified and acknowledges, the doctor sees an urgent flag | Add reading → Critical alert → Parent view → Doctor panel | FR-05, FR-07, FR-10, NFR-08 | E2E-3 |
| S3 | Patient sees tonight’s low risk with its reasons | Home (risk card); Doctor panel (factor bars) | FR-14, FR-15 | Eval. 3.1 |
| S4 | Patient reviews the week and repeated patterns | Trends | FR-16 | Eval. 3.2 |
| S5 | Patient asks the coach about own data and asks for an insulin dose; the dose request is refused | Coach | FR-17, NFR-07 | E2E-5 |
| S6 | Patient invites a parent or doctor with a code and stops sharing with the doctor | Sharing | FR-08, FR-09, NFR-04, NFR-05 | E2E-1, E2E-4 |
| S7 | Doctor reviews the weekly report; a patient without readings shows “No data” | Doctor panel | FR-11, FR-12 | E2E-6 |

## 3. Screens

<p><img src="../prototype/01-Main.png" width="300" alt="Figure 6 – Patient app: home screen and adding a reading (S1, S2, S3)"> <img src="../prototype/02-AddReading.png" width="300" alt="Figure 6 – Patient app: home screen and adding a reading (S1, S2, S3)"></p>

*Figure 6 – Patient app: home screen and adding a reading (S1, S2, S3)*

Home (a) shows the latest reading with its range, tonight’s low-risk card with the reasons and a note that it is an estimate, the day’s summary and one main action, “Add reading”. Add reading (b) asks only for the value and the “When?” tag; carbohydrates, insulin and symptoms are optional. After saving, the value is classified by the default thresholds. An implausible value (for example 48) is not saved silently: the user is asked to measure again and check the units (Verification plan 2.3). A value below 3.0 mmol/L opens the alert screen.

<p><img src="../prototype/04-Alert.png" width="300" alt="Figure 7 – Critical low: patient alert screen and parent view (S2)"> <img src="../prototype/07-Parent.png" width="300" alt="Figure 7 – Critical low: patient alert screen and parent view (S2)"></p>

*Figure 7 – Critical low: patient alert screen and parent view (S2)*

The alert screen (a) is shown on the phone even without internet. It gives one clear action, shows who was notified (this phone, the parent, the doctor’s urgent flag), offers a 15-minute re-check reminder and emergency numbers. The parent view (b) is read-only: it shows the critical alert with the child’s reaction, the “I am on it” acknowledgement, today’s readings, the time of the last sync and tonight’s risk.

<p><img src="../prototype/03-History.png" width="300" alt="Figure 8 – Patient app: trends and AI coach (S4, S5)"> <img src="../prototype/05-Coach.png" width="300" alt="Figure 8 – Patient app: trends and AI coach (S4, S5)"></p>

*Figure 8 – Patient app: trends and AI coach (S4, S5)*

Trends (a) show 7 days of meter readings against the target band, the share of readings in range (not “time in range”, because meter data are sparse) and patterns in plain language. The coach (b) explains the patient’s own data and general facts. Each answer shows its basis (“Based on your logs”, “Source: ADA Standards of Care”). A request for an insulin dose is refused with the label “Safety rule: no dose advice, no diagnosis”; symptoms such as dizziness lead to “measure now”.

<p><img src="../prototype/06-Sharing.png" width="300" alt="Figure 9 – Patient app: access and consent (S6)"></p>

*Figure 9 – Patient app: access and consent (S6)*

The sharing screen lists everyone who can see the data and exactly what they see. The patient creates a one-time invite code (valid 24 hours) for a parent or a doctor and can stop sharing at any time. The access history shows each view of the data and each consent change, which comes from the audit log (NFR-05). For a patient under 18 the parent link cannot be removed from the app; this proposed rule will be checked with the supervisor and the doctor.

<p><img src="../prototype/08-Doctor.png" width="900" alt="Figure 10 – Doctor web panel: weekly report (S2, S3, S7)"></p>

*Figure 10 – Doctor web panel: weekly report (S2, S3, S7)*

The doctor panel follows the report content asked for by the general practitioner (D2): readings in range, low and high readings, average, a 7-day chart and a short summary for the visit. Patients are sorted by status (Urgent, Review, No data, Stable). The risk card shows the contribution of each factor and, during evaluation, the rule baseline result next to the model. The header states that access is read-only and that every view is logged; the doctor does not monitor patients in real time (FR-12).

## 4. Interaction with the backend and external systems

The screens exchange JSON with the REST API (Figure 3). Table 2 gives examples for scenario S2.

*Table 2 – Example requests and data for scenario S2*

| Step | Request / message | Response / content |
| --- | --- | --- |
| Save reading (mobile app → API) | <code>POST /api/v1/readings<br>Authorization: Bearer &lt;JWT&gt;<br>{&quot;client_uuid&quot;: &quot;7f3c…&quot;,<br> &quot;value&quot;: 2.8, &quot;unit&quot;: &quot;mmol/L&quot;,<br> &quot;tag&quot;: &quot;bedtime&quot;,<br> &quot;symptoms&quot;: [&quot;shaky&quot;],<br> &quot;measured_at&quot;: &quot;2026-10-19T22:01:00+05:00&quot;}</code> | 201 Created<br>{"id": "r_1842",<br> "level": "urgent_low",<br> "alert_id": "a_311",<br> "notified": ["parent"]} |
| Push to parent (backend → FCM) | <code>title: &quot;Critical low · Aliya&quot;<br>body: &quot;2.8 mmol/L at 22:01&quot;<br>data: {&quot;alert_id&quot;: &quot;a_311&quot;}</code> | No reading history and no diagnosis in the push; the app loads details after login |
| Weekly report (doctor panel → API) | <code>GET /api/v1/patients/{id}/report?week=2026-W42<br>Authorization: Bearer &lt;JWT, role = doctor&gt;</code> | 200 OK with in-range share, lows, highs, average, chart points, risk factors, urgent flag;<br>403 if consent was revoked |
| Coach question (backend → LLM) | <code>Question with names and IDs removed + ADA guideline excerpts</code> | Draft answer, checked by the output filter before it is shown |

## 5. Coverage and next steps

The prototype covers the Must scenarios of the patient, the parent and the doctor. The following screens are not mocked yet and will be added before pre-defense 2:

- **Onboarding:** registration and profile (diabetes type, treatment type, units, language; FR-01, FR-20).
- **Parent data entry:** entering data by a parent for a young child (FR-10).
- **Meal tools:** meal bolus calculator with the low-glucose block and maximum dose (FR-13), and food photo recognition (FR-22, Could).
- **Settings:** alert thresholds, quiet hours and reminders (FR-06, FR-18); export and deletion of data (FR-21).
- **Admin console:** user and role management and the audit log viewer.

Validation plan: the general practitioner from the interviews has agreed to review the mockups and a sample report; the usability test with 5 participants (NFR-10: a reading logged in 3 taps or fewer and under 15 s) will be run on the Kazakh and Russian versions.
