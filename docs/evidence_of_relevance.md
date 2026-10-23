# Evidence of the Relevance

The requirements of Diabetes AI Assistant are based on three sources: an online survey of 20 parents of children with type 1 diabetes, a face-to-face interview with a young adult patient, and an interview with a general practitioner. The evidence confirms the core of the project. Most parents check their child's glucose at night (15 of 20), only 4 of 20 can see the child's readings remotely, 16 of 20 want an app that warns them before glucose falls, and glucose prediction is the most requested feature (15 of 20). The doctor confirmed the need for a short report for the doctor, simple and configurable alerts and strict limits on AI advice. Some ideas are supported by only one source, for example photo-based carbohydrate counting, so they are listed as assumptions in Section 5.

## 1. Sources of Evidence

| ID | Source | Participants | Method | Date | Status |
| --- | --- | --- | --- | --- | --- |
| E1 | Online survey of parents (Google Form, Kazakh and Russian, 23 questions) | 20 parents and caregivers of children with type 1 diabetes aged 2–18 | Closed and open questions: daily routine, night checks, remote monitoring, worries, features, trust in AI, privacy, price | Oct 2026 | Done |
| E2 | Interview with a patient | 1 young adult (18 years), type 1 diabetes for 6 years | Face-to-face interview with a prepared script of 20 questions; answers recorded in Kazakh | Oct 2026 | Done |
| E3 | Interview with a general practitioner (DIP-4) | 1 general practitioner, primary care | 9 open questions sent by messenger after a short project description; answers received as voice messages and transcribed (Russian) | Oct 2026 | Done |

Ethics: all 20 survey respondents gave consent to take part before answering. Answers are anonymised (respondents are numbered P1–P20), no names or contact details of respondents, the patient or the doctor are reported, and the raw answers are kept outside the Git repository (NFR-06).

## 2. Survey of Parents (E1)

### 2.1 Who answered

| Characteristic | Result (n = 20) |
| --- | --- |
| Diabetes type | Type 1: 20 of 20 |
| Age of the child | 2–6 years: 5; 7–12 years: 6; 13–18 years: 9 |
| Treatment and devices (several answers allowed) | Insulin pen: 13; insulin pump: 9; glucometer: 13; CGM sensor (Libre, Dexcom, etc.): 9; diet: 5 |
| Who mainly monitors glucose | Mother: 11; the child: 6; father: 2; whole family: 1 |
| Time spent per day | Up to 30 min: 11; 30–60 min: 1; 1–2 hours: 4; more than 2 hours: 4 |
| What the child does alone | Measures glucose: 15; injects insulin: 13; counts carbohydrates: 9; none of these (the youngest children): 5 |

### 2.2 Key findings

| # | Finding | Data | Meaning for the project |
| --- | --- | --- | --- |
| F1 | Night monitoring is a daily burden | 15 of 20 wake up to check glucose every night (7) or several times a night (8); only 2 never do. Two parents described night hypoglycemia with very low readings when the child had to be woken and given juice. | Night hypo alerts to the parent are a core feature, not an extra |
| F2 | Most parents cannot see readings remotely | Only 4 of 20 view the child's data remotely; 13 do not. Parents learn school readings by calling the child or going to the school. A sensor user noted that the mother sees nothing when the child's phone has no internet. | Parent account with remote view and push alerts |
| F3 | Many families have no CGM | 11 of 20 do not use a CGM sensor; 13 use a glucometer. Several parents say sensors are bought with their own money. | Manual entry of readings in the MVP is justified; CGM support later |
| F4 | Parents want early warnings | 16 of 20 answered “Yes” and 4 “Maybe” to an app that warns the parent before glucose falls; nobody answered “No”. | Confirms risk scoring and NFR-08 (fast alerts) |
| F5 | Glucose prediction is the top feature | Glucose forecast: 15; nutrition advice: 11; reminders: 11; weekly report: 9; contact with the doctor: 9. | Priority order of AI features |
| F6 | Too many alerts are a problem | Parents want alerts on high and low glucose; one parent complained that the device beeps during the child's lessons. | Alert thresholds and quiet hours set by the user |
| F7 | Privacy and the child's control matter | Data security rated 5 of 5 by 17 of 20; the child controlling what parents see rated 5 of 5 by 14 of 19. | Confirms NFR-03, NFR-04 (consent, revocable access) |
| F8 | Trust in AI is conditional | 9 would trust AI advice; 6 only with conditions (“if it is not wrong”, “yes, but I will still check”); 1 would not; 4 unsure. Accuracy is the most common advice to the team. | Recommendation-only mode, every warning shows its reason (NFR-07) |
| F9 | Gaps in the health system | 5 report shortages of test strips and sensors (e.g. two strips a month from the clinic); 8 mention a lack of type 1 diabetes knowledge among doctors and school staff, including confusion between type 1 and type 2 and no paediatric endocrinologist outside the regional centre. | Doctor link and simple explanations for school staff and parents |
| F10 | Willingness to pay is mixed | More than 5,000 ₸/month: 9; 3,000–5,000 ₸: 3; 1,000–3,000 ₸: 3; under 1,000 ₸: 1; only free: 4. | Freemium: safety features free, extras paid |

## 3. Interview with a Patient (E2)

The interview was held face to face using a prepared script of 20 questions. The respondent is 18 years old and has had type 1 diabetes for 6 years. The patient uses insulin pens, checks glucose with a glucometer 6–7 times a day and sometimes uses a sensor (Ottai, Libre).

- **Logging:** the patient kept a paper diary but stopped, because writing was often forgotten and the notebook was hard to carry. No diabetes app is used today.
- **Carbohydrate counting:** the patient reads carbohydrates on the package and divides by 12 (1 bread unit = 12 g). In cafés the patient sends a photo of the meal to a website that estimates the carbohydrates.
- **Hardest part:** keeping glucose in range. Hypoglycemia happens more often than hyperglycemia. The last severe case was at night after a tiring day: the patient woke up sweating and shaking with glucose at 2.3 mmol/L and could not sleep well afterwards.
- **Daily risks:** with friends the patient sometimes forgets to count insulin before eating. Stress raises glucose even with correct doses, so the patient changes the bolus ratio (1 bread unit = 1.5 units instead of 1 unit).
- **Doctor visits:** the patient visits the endocrinologist every 3 months and brings glucose readings and test results.
- **Priorities and trust:** photo-based carbohydrate counting first, then glucose prediction. The patient would trust AI after checking it personally 1–2 times, and real experience of other patients would help.
- **Other:** data privacy is “very important”, the preferred language is Kazakh, and the patient would pay about 10,000 ₸ for one app that combines the tools now used on separate websites.

## 4. Interview with a General Practitioner (E3)

Before the questions, the doctor received a short description of the app: manual entry of readings, meals, medication and activity; charts and simple explanations; alerts on critical values (to parents for minors); doctor access only by invite code with the patient's consent; no diagnoses and no insulin doses. The doctor mostly sees patients with type 2 diabetes and refers patients with poor control, insulin selection or complications to an endocrinologist.

| # | Finding | What the doctor said | Meaning for the project |
| --- | --- | --- | --- |
| D1 | Patient data is incomplete | Patients bring notes in a notebook, photos of notes, glucometer data or tell values from memory. Often only fasting glucose is measured, so readings after meals and changes during the day are unknown. | Manual entry with meal tags (fasting / after meal) and medication intake |
| D2 | The doctor needs a short report | A chart for a week or a month, average, minimum and maximum, number of hypo- and hyperglycemia episodes, how regularly the patient measures, HbA1c if added and the list of medications. The report should show quickly whether there is a problem. | Content of the weekly report in the doctor panel |
| D3 | Access by invite code is acceptable | The doctor would consider it if the patient gives access, to review data before the visit. Concerns: privacy, access security, reliability of data, extra workload; the app must not suggest that the doctor watches the patient 24/7. | Confirms NFR-03, NFR-04; the app states that the doctor does not monitor in real time |
| D4 | Why patients stop measuring | Forgetting, low motivation, no habit, tiredness after many years, difficulty with apps and devices, cost of test strips. Reminders, automatic saving and showing the link between habits and glucose would help. | Reminders; trend explanations in simple language |
| D5 | Alert thresholds | Below 3.9 mmol/L as a hypoglycemia warning, below 3.0 mmol/L as clinically significant hypoglycemia. High thresholds depend on the diabetes type and personal targets. For suspected severe hypoglycemia or ketoacidosis the app should give clear steps and advise urgent medical help. Alerts must be configurable and not too frequent. | Confirms the 3.9 mmol/L baseline (DIP-11); adds a 3.0 mmol/L level and an emergency action screen |
| D6 | Limits of AI advice | Main risk: the patient treats AI advice as a prescription and changes the dose. The app may explain values, remind about medication and measurements, give general advice on food and activity and suggest re-checking or seeing a doctor. It must not change doses, prescribe or cancel drugs, or diagnose. The AI should explain why it gives advice and state its limits. | Confirms NFR-07 and the AI safety test set |
| D7 | Type 1 and type 2 need different features | Type 1: real-time control, hypo/hyper alerts, CGM. Type 2: adherence to treatment, reminders about check-ups; there are many such patients in primary care. The app should know whether the patient uses insulin or tablets. | Profile stores diabetes type and treatment type; type 2 stays with basic features |
| D8 | Keep it simple | Too many functions, charts and alerts make the app hard to use. Simple interface, large font, clear instructions, manual entry for patients without devices. Useful addition: reminders for regular check-ups (HbA1c, kidney tests, eye exam). | NFR-10 usability; check-up reminders as a “Could” feature |
| D9 | Who else to ask | Endocrinologists, nurses, patients on insulin and on tablets, paediatric endocrinologists and parents of children. The doctor agreed to give feedback later on mockups and sample reports. | Next interviews; review of mockups |

## 5. Verified Findings and Assumptions

A finding is “verified” when it is supported by two sources or by several respondents of one source, “partly verified” when it comes from one person or is indirect, and an “assumption” when there is no evidence yet.

| Statement | Status | Evidence | How we will check it |
| --- | --- | --- | --- |
| Parents check glucose at night and want early warnings | Verified | E1: F1, F4; E3: D5 | — |
| Most parents cannot see the child's data remotely | Verified | E1: F2 | — |
| Many users rely on a glucometer, so manual entry is needed | Verified | E1: F3; E2; E3: D1, D8 | — |
| Glucose prediction is the most wanted feature | Verified | E1: F5; E2 (second priority) | — |
| Alerts must be configurable and not too frequent | Verified | E1: F6; E3: D5, D8 | — |
| Privacy and the child's control over data are important | Verified | E1: F7; E2; E3: D3 | — |
| AI gives advice only, explains it, and never gives doses or diagnoses | Verified | E1: F8; E2; E3: D6 | AI safety test set (DIP-11) |
| Meal bolus calculator with a doctor-set ratio is acceptable | Assumption | interview with endocrinologist |  |
| A short report helps doctor visits | Verified | E1: 9 of 20; E2 (visits every 3 months); E3: D2 | Feedback from the doctor on a sample report |
| Hypoglycemia thresholds 3.9 and 3.0 mmol/L | Partly verified | E3: D5 (one doctor) | Endocrinologist review |
| Doctors will use the web panel | Partly verified | E3: D3 (“would consider it”), concerns about workload | Doctor review of mockups |
| Photo-based carbohydrate counting is a priority | Partly verified | E2 only; E1 had no question about it (nutrition advice: 11 of 20) | Keep the feature optional |
| The interface must be in Kazakh and Russian | Partly verified | E2 prefers Kazakh; E1 respondents answered in both languages | Usability tests (NFR-10) |
| Needs of type 2 patients | Partly verified | E3: D7 (doctor's view); no type 2 patients asked | Type 2 kept to basic features |
| The AI model warns about hypoglycemia earlier than fixed thresholds | Assumption | — | Evaluation on data (DIP-11) |
| Sharing readings must work with a weak internet connection | Assumption | One parent in E1 | Further interviews |
| Users will actually pay the price they named | Assumption | Stated, not observed | Not checked in this project |

## 6. Impact on the Requirements

| Finding | Effect on the requirements |
| --- | --- |
| F1, F2, F4, D5 | Confirms: parent account with remote view and push alerts on low and high values (NFR-08) |
| F3, D1, D8, E2 paper diary | Confirms: manual entry in the MVP with meal tags and medication intake; logging must be fast (NFR-10: 3 taps, under 15 s) |
| F5 | Confirms: glucose risk prediction is the main AI module |
| D5 | New (proposed): two hypoglycemia levels (below 3.9 and below 3.0 mmol/L) and an emergency screen with clear steps and a call for help |
| F6, D5, D8 | New (proposed): user-set alert thresholds and quiet hours |
| F7, D3 | Confirms NFR-03, NFR-04, NFR-05; new (proposed): a teenager chooses which data types the parent sees |
| F8, D6 | Confirms NFR-07; every risk warning shows the reason (e.g. “glucose fell 2 mmol/L in the last hour”) |
| D2 | Weekly report: chart, average, min/max, hypo/hyper episodes, measurement regularity, HbA1c, medications |
| D3 | Doctor panel states that the doctor does not monitor the patient in real time |
| D4, E2: missed bolus, stress | New (proposed): reminders; tags for stress, sport and sleep on each reading, also used as model features |
| D7 | Profile stores treatment type (insulin or tablets) in addition to diabetes type |
| D8 | Could: reminders for regular check-ups (HbA1c, kidney tests, eye exam); simple interface with large font |
| E2, E1 languages | New (proposed): Kazakh and Russian interface |
| F10 | Business model: core safety features free |

## 7. Limitations and Next Steps

- The survey is a small convenience sample (20 respondents); results show trends, not exact shares for all families in Kazakhstan.
- All survey respondents are parents of children with type 1 diabetes; there are no type 2 patients and only one adult patient.
- Only one doctor was interviewed, and a general practitioner mostly works with type 2 diabetes; an endocrinologist has not been interviewed yet.
- Answers are self-reported; answers in Kazakh and Russian were translated into English by the team.
- Next steps: interview an endocrinologist (preferably paediatric), and show mockups and a sample weekly report to the general practitioner, who agreed to give feedback.
