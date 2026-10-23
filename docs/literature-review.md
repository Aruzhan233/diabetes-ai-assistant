# Literature Review

## 1. Scope and source selection

This review analyses 21 sources and one legal act to answer three questions: what approaches exist for AI-supported diabetes monitoring, what is already known about their effectiveness and safety, and which limitations our project must take into account.

The project builds a mobile application (Flutter) with a backend that (a) records glucose readings,(b) predicts the risk of nocturnal hypoglycaemia at bedtime from sparse meter readings with a machine learning model evaluated on the CITY dataset, (c) applies rule-based thresholds to every new reading, and (d) supports the user with educational content. The review is organised around these components.

### Inclusion criteria

- Peer-reviewed journal articles, randomised controlled trials (RCTs), systematic reviews, meta-analyses, official clinical guidelines, or national legislation on personal data.
- Topic: type 1 diabetes (T1D) monitoring, glucose prediction, explainable AI, diabetes mobile apps, decision support, large language models (LLMs) in diabetes care, remote monitoring or health-data privacy.
- Published 2022–2026, with older works retained only when they are foundational (the consensus that defines CGM targets, the trial that produced our dataset, the first work on predicting nocturnal hypoglycaemia from meter readings and a meta-analysis of prediction accuracy).

**Search keywords:** "type 1 diabetes" AND ("continuous glucose monitoring" OR "hypoglycemia prediction" OR "machine learning" OR "explainable" OR "SHAP" OR "mobile app" OR "decision support" OR "large language model" OR "remote monitoring" OR "data privacy").

**Databases searched:** PubMed, Google Scholar and IEEE Xplore. Clinical guidelines were taken from the official ADA journal site (diabetesjournals.org) and the Kazakhstan law from the official legal database (adilet.zan.kz).

*Table 1 – Resulting corpus*

| Measure | Value |
| --- | --- |
| Sources used | 22 (21 publications + 1 legal act) |
| Scientific publications (journal articles, RCTs, reviews) | 17 |
| Official clinical guidelines / consensus reports | 4 (ADA 2025 Sections 6 and 14; International Consensus on Time in Range 2019; ISPAD 2022) |
| Legal acts | 1 (Law of the Republic of Kazakhstan No. 94-V "On Personal Data and Their Protection", as amended in 2026) |
| Publications from the last five years (2022–2026) | 16 of 21 |
| Foundational works retained | 5 (Battelino 2019; Laffel 2020; Fremont 2021; Kodama 2021; Sampath 2016) |
| Randomised controlled trials or meta-analyses of RCTs | 3 (Laffel 2020; Kobayati 2025; Pi 2023) |

The requirement of at least 12 meaningfully used sources, at least 8 of them scientific publications, is therefore met. Every source informs at least one project decision; the last column of the table in Section 9 shows which.

## 2. Clinical background: what the application must measure

The clinical thresholds the application uses are not our own choice: they come from the American Diabetes Association (ADA) Standards of Care 2025 [1], [2] and the International Consensus on Time in Range [3]. Using them makes the app's alerts and statistics clinically interpretable and defensible.

*Table 2 – Glycaemic bands used by the application (ADA 2025, Section 6 [1]; Battelino et al. [3])*

| Band | Glucose | Clinical meaning | Target share of time (most adults with T1D) |
| --- | --- | --- | --- |
| Level 2 hypoglycaemia | \< 54 mg/dL (\< 3.0 mmol/L) | Clinically significant; needs immediate action | \< 1% |
| Level 1 hypoglycaemia | 54–69 mg/dL (3.0–3.8 mmol/L) | Alert value | \< 4% (both levels combined) |
| Time in range (TIR) | 70–180 mg/dL (3.9–10.0 mmol/L) | Target range | \> 70% |
| Level 1 hyperglycaemia | 181–250 mg/dL (10.1–13.9 mmol/L) | Above range | \< 25% (both levels combined) |
| Level 2 hyperglycaemia | \> 250 mg/dL (\> 13.9 mmol/L) | Very high | \< 5% |

### What is already known

- **A1C is no longer enough.** ADA 2025 recommends an A1C goal of \< 7% for many adults and youth, but recommends CGM metrics (TIR, time below range, time above range) alongside it, because A1C hides variability and hypoglycaemia [1], [2]. A 70% TIR corresponds approximately to an A1C of 7% [1].
- **Standardised reporting exists.** The consensus defines 10 core CGM metrics, including at least 14 days of data with ≥ 70% sensor wear, a glycaemic variability target (coefficient of variation ≤ 36%) and the ambulatory glucose profile report [3]. Each 5% increase in TIR is associated with clinically significant benefits [3].
- **Hypoglycaemia has a defined first-aid rule.** For most people, 15 g of carbohydrate should be taken and glucose rechecked after 15 minutes [1]. A rule-based module for manual readings can implement this deterministically.
- **Youth need a family-centred approach.** ADA Section 14 states that self-management in youth involves both the young person and their parents or caregivers, recommends glucose monitoring up to 10 times per day, offering CGM as soon as possible, and screening for diabetes distress [2]. For people under 25 with an A1C goal of 7.5%, the consensus sets the TIR target at about 60% [3].

### Limitations taken into account

- The application will not set personal targets itself. Default thresholds follow ADA/consensus values; individualised goals (e.g. A1C \< 7.5% or \< 8%) are a clinician's decision [2], so targets must be editable rather than hard-coded.
- Manual finger-stick readings cannot produce TIR, which requires near-continuous data [3]. For manual-entry users the app will report readings in range and flag hypoglycaemic values, not a pseudo-TIR.
- The app is a self-management support tool, not a medical device; any alert must point the user to the standard treatment rule and to their care team.

## 3. The CITY trial: origin and limits of our training data

Our ML model is trained on data from the CITY trial (CGM Intervention in Teens and Young Adults with T1D) [4], so the trial's design defines what the model can and cannot learn. It is the most important source for justifying the dataset choice to the committee.

### Trial design [4]

- Randomised clinical trial, 14 US endocrinology centres, January 2018 – May 2019, published in JAMA (2020).
- 153 participants aged 14–24 years with T1D and HbA1c 7.5–10.9%; mean age 17, 50% female, mean diabetes duration 9 years.
- CGM group (n = 74): Dexcom G5 real-time CGM, glucose every 5 minutes, twice-daily finger-stick calibration. Control group (n = 79): usual blood glucose meter monitoring, with periodic masked (blinded) CGM so outcomes could be measured.
- 26-week intervention, 142 participants (93%) completed it.

### Key findings [4]

- HbA1c fell from 8.9% to 8.5% with CGM and stayed at 8.9% with meter monitoring (adjusted difference −0.37%, 95% CI −0.66 to −0.08; p = .01).
- Time in range rose from 37% to 43% with CGM (difference 6.9 percentage points, p \< .001); time below 70 mg/dL and below 54 mg/dL also fell.
- Use declined over time: 82% used CGM ≥ 5 days/week at week 6, only 68% by month 6.
- Even with CGM, mean TIR (43%) stayed far below the 70% target [1], [3], confirming that this age group has the worst glycaemic control across the lifespan.

### Why it suits our project

- Real-world CGM traces at 5-minute resolution from adolescents and young adults, the population with the highest rate of acute complications [4], [5].
- Paired CGM and meter arms make it possible to study both data types the app handles (continuous and manual readings).
- The dataset is publicly released by the Jaeb Center for Health Research and has a peer-reviewed trial behind it, so its collection protocol is documented.

### Limitations we take into account

- **Population bias.** Participants were mostly US, 59–66% White non-Hispanic, with HbA1c 7.5–10.9% [4]. The model may not generalise to Kazakhstani users, younger children or well-controlled adults; this must be stated as a known limitation and tested if local data become available.
- **Older sensor.** The Dexcom G5 required calibration; current factory-calibrated sensors behave differently [4]. Prediction performance on today's sensors is not guaranteed.
- **Short duration and selection.** Only 26 weeks, and the run-in phase may have excluded less adherent users [4]. Real app users will have more gaps and missing data.
- **Few hypoglycaemic events.** Time below 70 mg/dL was only 2–4% [4], so hypoglycaemia is a rare class. The model must be trained and evaluated with class imbalance in mind (see Section 4).
- **Simulated meter input.** CITY contains CGM data, while our users enter meter readings. Model inputs are therefore sampled from the CGM series at realistic times; real measurement timing differs between people, so several sampling scenarios are tested. Meal, insulin and activity features are used only where CITY records them; otherwise this is reported as a limitation [6], [21].

## 4. Machine learning for nocturnal hypoglycaemia prediction

Most users of the app will enter 4–7 meter readings per day, and parents are most worried about the night. The model therefore predicts at bedtime whether nocturnal hypoglycaemia will occur during the next 8 hours, using only sparse self-monitoring data. The literature provides three building blocks: prediction from intermittent meter readings [21], machine learning on the CITY dataset [5], and evidence on how accurate hypoglycaemia prediction is in general and at night [7], [22].

### Foundation 1 — nocturnal hypoglycaemia from intermittent readings (Sampath et al., 2016) [21]

- Developed on 34 adults with T1D (DIAdvisor dataset) using four meter readings per day; nocturnal events were confirmed by finger-stick.

- Four glycaemic control indices (LBGI, the last bedtime reading, the Hypoglycaemic Index and the GRADE Hypoglycaemic Index) were each turned into a predictor and combined by linear aggregation.

- Adults: sensitivity 80%, specificity 96.4%. Without recalibration, on 179 children (476 records) from Kyiv hospitals: sensitivity 77%, specificity 83.5%.

- Limitations: a proof of concept; no meal, insulin or activity data; child data had only discrete readings, so some lows may have been missed.

### Foundation 2 — explainable machine learning on CITY data (Duckworth et al., 2022) [5]

- Used exactly our dataset: CGM data from all 153 CITY participants, more than 28,000 days.

- Predicted hypoglycaemia (\< 70 mg/dL) and hyperglycaemia (\> 270 mg/dL) up to 60 minutes ahead from dense CGM data, with features summarised over 1 hour, 1 day and 1 week.

- Data was split by person (75% / 25%), which prevents data leakage. Three models were compared (Table 3).

*Table 3 – Comparison of models for hypoglycaemia and hyperglycaemia prediction on CITY data [5]*

| Model | Hypoglycaemia AUROC | Hypoglycaemia average precision | Hyperglycaemia AUROC |
| --- | --- | --- | --- |
| Threshold heuristic (alert if glucose \< 110 or \> 240 mg/dL) | 0.811 | 0.121 | 0.734 |
| Logistic regression | 0.930 | 0.244 | 0.862 |
| XGBoost | 0.998 | 0.953 | 0.989 |

The key insight is the gap in average precision (0.121 vs 0.953): because hypoglycaemia is rare, AUROC looks good even for weak models, while average precision shows how many alerts would be false [5].

### How accurate is hypoglycaemia prediction? (Kodama et al., 2021; Katsarou et al., 2025) [22], [7]

- A meta-analysis of 33 studies (19 on prediction) found pooled sensitivity 0.80 and specificity 0.92 for predicting hypoglycaemia; for the 7 studies on nocturnal prediction, accuracy was lower (sensitivity 0.74, specificity 0.81) [22]. Heterogeneity was very high, and most studies used retrospective data.

- In 29 adults, an SVR + XGBoost ensemble reduced erroneous hypoglycaemia predictions at 30 minutes to 19%; the LSTM did not outperform simpler models, as deep learning usually needs larger datasets [7].

### Critical comparison

- Sparse data carry a useful signal. Sampath et al. reached about 77–80% sensitivity from four readings per day [21], close to the pooled sensitivity of models that mostly use richer data [22].

- Night is the hardest case: nocturnal prediction is less accurate than prediction in general [22], so modest results must be expected and reported honestly.

- The two foundations complement each other. Sampath et al. used sparse readings but fixed indices and no meal or insulin data [21]; Duckworth et al. used gradient boosting and SHAP, but on dense CGM data and a 60-minute horizon [5]. No reviewed study combines gradient boosting with sparse meter data in adolescents and young adults; this is the research question of our project.

- Overall accuracy is misleading for safety: models must be judged on the rare class (average precision, errors in the hypoglycaemic region), not only by AUROC [5], [7].

- Simple baselines are still predictive: a threshold rule reached an AUROC of 0.811 [5], so a rule baseline is a meaningful comparison, not a placeholder.

- Published results depend on clean data: Duckworth et al. required ≥ 80% CGM wear [5]. Real meter data will have gaps, so the evaluation must simulate realistic sampling.

- Deep learning is not automatically better on small datasets [7], and sparse meter data are unsuitable for sequence models such as LSTM.

### Decisions for the project

1.  Prediction task: at bedtime, predict whether nocturnal hypoglycaemia (glucose \< 3.9 mmol/L for at least 15 minutes [1]) will occur during the next 8 hours; Level 2 events (\< 3.0 mmol/L) are reported separately. Current values are classified by fixed thresholds [1], [3]; hyperglycaemia is handled by rule-based alerts only.

2.  Data: labels are computed from the dense CITY CGM series; model inputs are built only from simulated meter readings (4–7 per day at realistic times, with meter measurement error), so the evaluation matches how the app is used [4], [21].

3.  Model: gradient boosting (XGBoost / LightGBM), compared with a rule baseline, logistic regression and the same model trained on dense CGM features as an upper reference, which keeps our results connected to [5].

4.  Evaluation: patient-level split; recall as the main metric, plus AUPRC and false alarms per patient-night [5], [22].

5.  Data-sufficiency rule: with too few recent readings, the app shows “Not enough data” instead of a low-risk result.

## 5. Explainable AI: why accuracy alone is not enough

In health applications a model must be not only accurate but also physiologically sound and explainable, because two models with the same accuracy can lead to different, and sometimes harmful, decisions [6]. SHAP (SHapley Additive exPlanations) is the dominant method for this [8], and it works natively with tree models such as XGBoost.

### Evidence that explanation changes the model choice (Prendin et al., 2023) [6]

- Two LSTM networks predicted glucose from CGM, insulin and carbohydrate data (OhioT1DM dataset). Their accuracy was almost identical: RMSE at 30 minutes was 21.43 mg/dL for the non-physiological LSTM and 21.67 mg/dL for the physiological LSTM.
- SHAP showed that the slightly "better" model had learned a physically wrong relationship: higher insulin predicted **higher** future glucose. Only the physiological LSTM learned that insulin lowers glucose and carbohydrates raise it.
- When each model was embedded in a decision support system that suggested corrective insulin, only the physiological model helped: time in range rose from 60.9% to 80.7% and time above range fell from 39.1% to 19.3% (30-minute horizon, simulated replay).
- Patient-entered data were unreliable: one subject logged only 31 of at least 141 expected meals. Poor manual logging directly damages what a model learns.

### State of XAI practice (Caterson et al., 2024) [8]

- Scoping review of 76 studies (2017–2023) applying ML with XAI to tabular electronic health records, following PRISMA guidelines; publications grew exponentially.
- XGBoost (51 studies) and Random Forest (50) were the most used models; SHAP was used in 63 of 76 studies, far ahead of partial dependence plots (11) and LIME (8).
- SHAP is the most theoretically grounded method and covers individual and global explanations, but authors rarely justified choosing it and rarely evaluated whether explanations were valid or robust.
- Explanations show association, not causation; correlated features can distort importance [8].

### Critical assessment

- Duckworth et al. [5] already applied SHAP to an XGBoost model on CITY and turned the explanations into per-user recommendations, which shows the approach is feasible for our dataset and model type.
- Prendin's study used a single patient [6] and Caterson's review excluded time-series data [8], so neither proves that SHAP explanations are clinically correct for CGM time series. They justify using SHAP as a **check**, not as ground truth.

### Decisions for the project

1. Use SHAP (TreeExplainer) on the XGBoost model to verify that feature effects are physiologically plausible before deployment, for example that a low bedtime glucose or a recent low increases the risk of nocturnal hypoglycaemia
2. Show users a short, plain-language reason for each risk alert (top contributing factors), not raw SHAP values.
3. Present explanations as associations ("your glucose has been falling quickly in the last hour") rather than causal claims or treatment instructions.
4. Keep manual-entry fields minimal, because incomplete self-reported data degrade any model [6].

## 6. Mobile health apps and decision support systems

The evidence is consistent: simple logging apps do not significantly improve glycaemic control, while apps that **actively support decisions** do [9]–[12]. This is the main argument for building an AI-assisted app rather than another glucose diary.

### Quality of existing diabetes apps (Geirhos et al., 2022) [9]

- 2,269 apps found in European app stores, 120 evaluated by two independent raters with the Mobile App Rating Scale (MARS).
- Overall quality was moderate (MARS 3.20 of 5). Functionality scored highest (4.10); engagement (2.80) and information quality (2.26) scored lowest.
- Only 8% of apps had any scientific evidence; most offered no psychosocial coping content.
- Apps used a median of only 3 persuasive system design principles (range 0–15). Dialogue support (reminders, praise, feedback) correlated strongly with quality (r = 0.64).
- Store star ratings correlated only weakly with expert-rated quality (r = 0.31), so user ratings are not a reliable quality indicator.
- Only 8.3% of apps were password-protected, although 80% showed a privacy policy.

### Effect of apps on young people with T1D (Pi et al., 2023) [11]

- Meta-analysis of 9 RCTs (748 participants, mean age 12.7–17.8 years, up to 24).
- Apps overall gave a **non-significant** HbA1c reduction (−0.26%, 95% CI −0.56 to 0.05; p = 0.10) and did not increase hypoglycaemia.
- "Auxiliary" apps with insulin or carbohydrate calculators significantly reduced HbA1c (−0.83%, 95% CI −1.10 to −0.56; p \< 0.001), whereas recording-only apps did not.
- Limitations: small trials (16–86 per intervention group), high heterogeneity (I² = 69%), mostly European and North American samples.

### Psychological well-being (Cloarec et al., 2025) [10]

- Systematic review: 8 of 2,142 screened studies met criteria (PROSPERO-registered).
- Six function categories were found: therapy, education, self-management, peer support, health professional–patient support and parental support.
- Only 2 of 8 apps improved psychological well-being, although about 1 in 3 people with T1D experience substantial diabetes distress. Most apps focus on glycaemia only.
- The authors call for theory-based design, stakeholder involvement and combined functions.

### AI decision support in an RCT (Kobayati et al., 2025) [12]

- 12-week randomised trial, 84 adults with T1D on multiple daily injections and HbA1c ≥ 7.5%, all using FreeStyle Libre sensors.
- A mobile app with a Bayesian titration algorithm gave weekly basal and meal-insulin recommendations; control used a non-adaptive bolus calculator app.
- HbA1c fell from 8.6% to 8.1% with the decision support system vs 8.6% to 8.5% in control (treatment effect −0.40%; p = 0.025), with no severe hypoglycaemia or ketoacidosis.
- Safety design: changes \> 30% were reviewed by clinicians before reaching the patient (50 cases, all approved); patients could override recommendations (41 of 504).
- Time in range did not differ between groups, and insulin doses were self-reported in the app, which the authors list as a source of error.

### Critical comparison

- Logging alone is not enough: recording-style apps showed no significant benefit [11]. The value lies in turning data into a timely, specific suggestion [11], [12].
- Engagement is the weak point of commercial apps [9], and CGM use itself dropped from 82% to 68% within six months in CITY [4]. An app that is not used cannot help.
- Even an effective RCT-tested decision support system improved HbA1c but not time in range [12], so claims for our MVP must be modest.
- Two kinds of dose support must be distinguished. Adaptive algorithms that change a patient’s insulin doses or ratios need clinician oversight and regulatory approval [12] and are outside the scope of a diploma project. A fixed meal bolus calculator that applies the ratio prescribed by the doctor is standard practice: it was the control condition in [12], and calculator-type apps were the only app group with a significant HbA1c effect [11].

### Decisions for the project

1. The app gives risk alerts and self-management guidance. It includes a meal bolus calculator (dose = carbohydrates / insulin-to-carbohydrate ratio) that uses only the ratio set by the doctor, is blocked when glucose is below 3.9 mmol/L [1] and limited by a maximum single dose. The app never changes ratios or recommends doses on its own, and the AI coach never gives doses [12], [20].
2. Include dialogue support features shown to raise quality: reminders to log readings, feedback on trends and positive reinforcement [9].
3. Educational content must follow ADA guidance [1], [2] and include coping with diabetes distress, not glucose only [10].
4. Evaluate the prototype with a structured usability instrument (e.g. MARS or the mHealth App Usability Questionnaire) rather than informal feedback [9], [12].

## 7. Large language models for diabetes education

Large language models (LLMs) give above-average answers to common diabetes questions, but about one in five answers in clinical stress tests is inappropriate [13]–[15]. Grounding the model in trusted documents (retrieval-augmented generation, RAG) measurably improves accuracy [13], so an LLM may be used for education only with RAG and strict scope limits.

### Answer quality of general LLMs (Ongen et al., 2025) [14]

- 20 real questions from children with T1D and their parents were posed to ChatGPT-3.5, ChatGPT-4, ChatGPT-4o, Gemini and Gemini Advanced.
- Five paediatric endocrinologists rated answers on the 5-point General Quality Scale: ChatGPT-4o scored highest (3.78), Gemini lowest (3.40); the difference was not significant (p = 0.103).
- The authors conclude that relying solely on LLMs is not adequate for a lifelong condition and recommend combining LLMs with health professionals, patients and parents.

### Improving accuracy with RAG (Wang et al., 2024) [13]

- The RISE framework rewrites the query, retrieves information from a curated knowledge base (guidelines, research papers), summarises it with fact-checking and then generates the answer.
- On 43 common diabetes questions, RISE raised the share of accurate answers by 12% on average across GPT-4, Claude 2 and Google Bard (by 7%, 19% and 9% respectively); comprehensiveness and patient-rated understandability also improved.
- Limitations: a small fixed set of questions, no clinical trial, and RAG quality depends on how current the knowledge base is.

### Safety risks found by red teaming (Chang et al., 2025) [15]

- 80 clinicians, students and engineers stress-tested GPT-3.5, GPT-4 and GPT-4 with internet access using 376 real clinical prompts (1,504 responses).
- 20.1% of responses were inappropriate (GPT-3.5: 25.8%; GPT-4: 16.0%; GPT-4 with internet: 17.8%); over half of these contained hallucinations.
- The newer GPT-4o was not safer (20.4% inappropriate), and 21.5% of answers that were appropriate with GPT-3.5 became inappropriate in updated models. Model updates can silently break previously safe behaviour.
- Models readily produced content with personal health information, showing little awareness of healthcare privacy standards.

### Critical comparison

- Expert-rated quality around 3.4–3.8 of 5 [14] means "useful but imperfect", not "safe to act on".
- RAG improves accuracy but does not remove errors: even with RISE, not all answers were accurate [13].
- None of the three studies tested Russian or Kazakh, the languages of our target users; Wang et al. themselves list multilingual performance as an open question [13]. This is a real limitation for our context.
- Because model behaviour changes between versions [15], a test passed once is not a guarantee; the evaluation must be repeatable.

### Decisions for the project

1. Any LLM feature is limited to **education** (explaining terms, ADA-based general guidance); it never gives insulin doses or diagnoses.
2. Use RAG over a curated knowledge base built from ADA Standards of Care [1], [2] and the Time in Range consensus [3], with sources shown to the user.
3. Build a fixed test set of diabetes questions (including Russian and Kazakh) and adversarial prompts, rerun it after every model or prompt change [15].
4. Never send identifiable personal data or raw glucose history to an external LLM API (see Section 8).
5. Every answer carries a reminder to consult the care team for treatment decisions [14].

## 8. Remote monitoring, caregivers and data privacy

Sharing glucose data with parents or partners brings clear benefits, but also anxiety and conflict when it is not controlled by the user [16], [17]. At the same time, glucose data are sensitive health data that consumer apps often fail to protect [9], [18]. Both findings shape how the app handles sharing and storage.

### Caregiver perspective (Polonsky & Fortmann, 2022) [17]

- Survey of 303 parents of children with T1D and 212 partners of adults with T1D using Dexcom real-time CGM data sharing.
- Most reported greater confidence about hypoglycaemia (97.7% of parents, 98.1% of partners) and better sleep (78.0% of parents, 61.3% of partners).
- A minority reported more anxiety (13.5% of parents) and more tension in the relationship (12.5% of parents).
- Benefits were greatest when caregivers celebrated success, encouraged during difficult periods, and agreed in advance how to respond to out-of-range values.
- Limitations: cross-sectional, self-reported, mostly White and college-educated respondents, and recruited via the device manufacturer, so positive results may be overestimated.

### Youth perspective (Fremont & Miller, 2021) [16]

- Separate interviews with 43 parents and 41 youth with T1D (mean age 13.8) two months after starting CGM.
- Four themes: peace of mind, parental anxiety, parent–child communication and technological limitations.
- Remote monitoring helped youth stay connected while learning self-management, but caused conflict through excessive texting and parental over-involvement. Families resolved this by agreeing boundaries.
- Some families could not use remote monitoring because the child had no smartphone.

### Data privacy and ownership (Dario, 2026) [18]

- Consumer CGM devices and direct-to-consumer apps often fall outside traditional health-data laws such as HIPAA; under GDPR, health data are a special category requiring explicit consent.
- In one cited analysis of 11 diabetes devices and apps in Europe, most sent data to third parties for purposes unrelated to the user's care.
- CGM traces are dense time series that can re-identify a person even after names are removed, so "de-identified" CGM data should be treated as identifiable.
- Recommended practices include data minimisation, privacy-enhancing technologies (federated learning, differential privacy), data portability and dynamic consent that users can change over time.

### Kazakhstan legal requirements (Law No. 94-V "On Personal Data and Their Protection", 2013, as amended) [19]

- The law applies to any owner or operator collecting personal data of people in Kazakhstan, so it governs our app regardless of the user's device or the CGM brand.
- **Localisation (Article 12, point 2):** personal data must be stored in a database and/or digital object located on the territory of the Republic of Kazakhstan.
- **Consent (Articles 8–10, 24):** data are collected and processed with the consent of the subject or their legal representative; access is limited to the terms of that consent; the subject may withdraw consent. Processing without consent is allowed only on the closed list of grounds in Article 9.
- **Cross-border transfer (Article 16):** allowed to countries that ensure protection of personal data; otherwise only on specific grounds, such as the subject's consent. Sending data to a foreign cloud or AI service is therefore a cross-border transfer.
- **Protection (Article 22):** the owner and operator must take legal, organisational and technical measures, including timely detection of unauthorised access; the duty lasts from collection until the data are destroyed or depersonalised.
- **Confidentiality (Article 11):** anyone who gains access to restricted-access personal data must keep it confidential; health data fall into this sensitive group.

### Critical comparison

- Evidence on sharing comes from the US and from manufacturer-linked samples [16], [17]; family dynamics and smartphone access in Kazakhstan may differ.
- Sharing reduces caregiver fear but can threaten adolescent autonomy [2], [16]; ADA also stresses a gradual transfer of responsibility to the youth [2]. The design must balance both.
- Privacy weaknesses are common in the existing market: only 8.3% of reviewed apps were password-protected [9]. Strong privacy can therefore be a real advantage of our app, not just a compliance item.
- International reviews focus on HIPAA and GDPR [18]. Kazakhstan's law is stricter on one point that directly shapes our architecture: personal data must be stored inside the country [19].

### Decisions for the project

1. Caregiver sharing is **opt-in, controlled by the patient**, revocable at any time, with selectable scope (e.g. alerts only vs full history) [16], [17].
2. Shared alerts include suggested supportive actions rather than blame, following the caregiver behaviours linked to better outcomes [17].
3. Collect only necessary data; no third-party analytics or advertising SDKs; encrypt data in transit (HTTPS/TLS) and at rest; authentication required [9], [18], [19].
4. Explicit, separate consent at registration with a clear privacy policy; for users under 18, consent of a parent or legal representative; withdrawal of consent, data export and deletion available in the app [18], [19].
5. The backend database is hosted on servers located in Kazakhstan, as required by Article 12 of Law No. 94-V [19].
6. No glucose history or identifiers are sent to foreign AI or analytics services; ML inference runs on our own backend, so no cross-border transfer occurs [15], [18], [19].

## 9. Structured analysis of publications

Each source is summarised by design, data, key finding, main limitation and how the project uses it. Numbers match the reference list.

*Table 4 – Structured analysis of publications*

| \# | Source | Type / design | Data / sample | Key finding | Main limitation | Use in our project |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | ADA, 2025 (Sec. 6) | Clinical guideline | Evidence-graded recommendations | Defines A1C \< 7%, TIR \> 70%, TBR \< 4%, hypoglycaemia levels 1–3, 15 g carbohydrate rule | Targets for adults; must be individualised | Alert thresholds, rule-based logic, educational content |
| 2 | ADA, 2025 (Sec. 14) | Clinical guideline | Paediatric/youth recommendations | Family-centred care, CGM for youth, glucose checks up to 10×/day, distress screening | US-oriented care model | Target users, caregiver role, education scope |
| 3 | Battelino et al., 2019 | International consensus | Expert panel | 10 core CGM metrics; ≥ 14 days, ≥ 70% wear; TIR targets incl. ~60% for under-25s | Foundational (pre-2021); evidence for some targets limited | Statistics screen, data-sufficiency rule |
| 4 | Laffel et al., 2020 (CITY) | RCT, 26 weeks | 153 people aged 14–24, Dexcom G5 | CGM lowered HbA1c by 0.37% and raised TIR from 37% to 43% | US sample; calibrated older sensor; HbA1c 7.5–10.9% only | Training dataset; population bias stated |
| 5 | Duckworth et al., 2022 | ML study | CITY, 153 people, \> 28,000 days | XGBoost predicts hypo/hyper up to 60 min (AUROC 0.998 / 0.989; AP 0.953) vs heuristic AP 0.121 | Requires ≥ 80% CGM wear; no insulin/meal data | Dataset handling, XGBoost, baselines, metrics, patient-level split; dense-CGM upper reference |
| 6 | Prendin et al., 2023 | XAI case study | OhioT1DM, 1 patient, replay simulation | Equal-accuracy LSTMs differed physiologically; only the sound one raised TIR (60.9% → 80.7%) | Single patient; simulated outcomes | SHAP plausibility check before deployment |
| 7 | Katsarou et al., 2025 | ML study | 29 adults, 2–4 weeks CGM | SVR + XGBoost ensemble cut erroneous hypo predictions to 19% at 30 min; LSTM not superior | Small sample; per-patient models | Evaluate errors in hypo region; 30-min input window |
| 8 | Caterson et al., 2024 | Scoping review (PRISMA) | 76 studies, 2017–2023 | SHAP used in 63 of 76 studies; XGBoost most common model; validity rarely evaluated | Tabular EHR only, not time series | Justifies SHAP; present explanations as associations |
| 9 | Geirhos et al., 2022 | Systematic app review | 120 apps (of 2,269), MARS | Moderate quality (3.20/5); 8% evidence-based; weak engagement and information quality | European stores only; snapshot in time | Persuasive design, evaluation instrument, privacy gap |
| 10 | Cloarec et al., 2025 | Systematic review | 8 studies (of 2,142) | Only 2 of 8 apps improved psychological well-being; 6 function categories | Few, heterogeneous studies | Include distress/coping content, involve users |
| 11 | Pi et al., 2023 | Meta-analysis of RCTs | 9 RCTs, 748 youth | Apps overall: HbA1c −0.26% (n.s.); calculator-type apps −0.83% (significant) | Small trials; I² = 69% | App must support decisions, not just log |
| 12 | Kobayati et al., 2025 | RCT, 12 weeks | 84 adults on MDI, FreeStyle Libre | Bayesian app-based DSS lowered HbA1c by 0.40% vs control, no severe hypoglycaemia | Adults only; self-reported doses; TIR unchanged | Scope: alerts not dosing; clinician-in-the-loop principle |
| 13 | Wang et al., 2024 | Comparative study | 43 questions, 3 LLMs | RAG (RISE) raised accurate answers by 12% on average | Fixed question set; no clinical trial | RAG over ADA guidelines for education |
| 14 | Ongen et al., 2025 | Cross-sectional comparison | 20 questions, 5 LLMs, 5 endocrinologists | LLM answers above average (3.40–3.78/5), no significant difference | Few questions; few models | LLM not used alone; refer to care team |
| 15 | Chang et al., 2025 | Red-teaming study | 376 prompts, 1,504 responses | 20.1% inappropriate, over half hallucinations; updates can break safe answers | General medicine, not diabetes-specific | Repeatable safety test set; no personal data to LLM |
| 16 | Fremont & Miller, 2021 | Qualitative interviews | 43 parents, 41 youth | Remote monitoring gives peace of mind but risks over-involvement | Single US centre; foundational (2021) | Patient-controlled, scoped sharing |
| 17 | Polonsky & Fortmann, 2022 | Cross-sectional survey | 303 parents, 212 partners | 97.7% more hypo confidence; 13.5% more anxiety; supportive actions predict benefit | Self-report; manufacturer-linked sample | Supportive alert wording for caregivers |
| 18 | Dario, 2026 | Narrative review | HIPAA, GDPR, EU frameworks | CGM data re-identifiable; many apps share data with third parties | No Kazakhstan law covered | Privacy-by-design requirements |
| 19 | Law of the RK No. 94-V, 2013 (as amended 2026) | Legal act | National personal data legislation | Requires consent, protection measures and storage of personal data in databases located in Kazakhstan; regulates cross-border transfer | Not health-specific; frequently amended, so the current consolidated text must be checked | Hosting in Kazakhstan, consent flow, no foreign AI data transfer |
| 20 | Cengiz et al., 2022 (ISPAD) | Clinical guideline | Expert consensus, evidence-graded recommendations for children and adolescents | Meal insulin is calculated from carbohydrate intake using an individual insulin-to-carbohydrate ratio; bolus calculators are recommended to reduce calculation errors | Ratios are individual and must be set and adjusted by the care team; part of the evidence is expert opinion | Bolus calculator formula; doctor-set ratio, low-glucose block and maximum dose |
| 21 | Sampath et al., 2016 | ML study | 34 adults (DIAdvisor) + 179 children; 4 meter readings/day | Aggregated glycaemic indices predicted nocturnal hypoglycaemia: SE 77%, SP 83.5% in children | Proof of concept; no meal, insulin or activity data | Foundation for prediction from sparse meter data; bedtime features |
| 22 | Kodama et al., 2021 | Meta-analysis | 33 studies (19 on prediction) | Pooled prediction SE 0.80, SP 0.92; nocturnal lower (SE 0.74, SP 0.81) | Very high heterogeneity; mostly retrospective data | Realistic accuracy expectations; recall as main metric |

## 10. Synthesis: research gap and limitations the project accounts for

The reviewed literature validates each component separately: nocturnal hypoglycaemia prediction from meter readings [21], explainable machine learning on CITY data [5], decision support in an app [11], [12], RAG-grounded education [13] and controlled data sharing [16], [17]. None of the reviewed works applies gradient boosting to sparse meter data for nocturnal prediction, combines it with parent alerts, or addresses Russian- and Kazakh-speaking users. This combination, delivered with privacy by design, is the gap our project targets.

### What is already known

1. CGM metrics (TIR, TBR, TAR) with standard thresholds are the accepted way to assess control [1], [3].
2. Adolescents and young adults have the worst control and benefit modestly from CGM [4].
3. Hypoglycaemia can be predicted with pooled sensitivity of about 0.80, but nocturnal prediction is less accurate (0.74) [22]; even sparse meter readings carry a predictive signal [21], and gradient-boosted trees outperform threshold rules mainly by reducing false alarms [5], [7].
4. Model explanations are necessary because equally accurate models can behave unsafely [6], [8].
5. Logging apps alone do not significantly improve HbA1c; decision-support features do [11], [12].
6. LLMs are useful for education but unsafe without grounding and oversight [13]–[15].
7. Data sharing helps caregivers but must respect youth autonomy and privacy [16]–[18].

*Table 5 – Limitations from the literature and how the project responds*

| Limitation found | Source | How the project accounts for it |
| --- | --- | --- |
| Training data from US youth with HbA1c 7.5–10.9% and an older sensor | [4], [5] | Stated as a limitation; no claims of accuracy for other populations; evaluation on held-out CITY participants only |
| Hypoglycaemia is rare, so AUROC overstates performance | [5], [7] | Average precision, sensitivity and errors in the hypo region reported as main metrics |
| Published accuracy assumes ≥ 80% CGM wear and clean data | [5] | Data-sufficiency check; fallback to rule-based alerts when data are missing Nocturnal prediction is less accurate than prediction in general \ | [22] \ | Recall as the main metric with confidence intervals; results reported even if the model does not beat the baseline The existing meter-based predictor is a proof of concept without meal or insulin data \ | [21] \ | Meal, insulin and activity features added where CITY records them |
| Equal accuracy can hide physiologically wrong models | [6] | SHAP plausibility check before release; plain-language reasons on alerts |
| Explanations are associations, not causes | [8] | Wording avoids causal and treatment claims |
| Manual logging is incomplete and error-prone | [6], [12] | Minimal input fields, reminders, rule-based handling of manual readings |
| Logging-only apps have no significant effect | [11] | App provides risk alerts, trend feedback and guidance, not just a diary |
| Low engagement and information quality in existing apps | [9], [10] | Persuasive design features; ADA-based content incl. diabetes distress; usability testing |
| Insulin dosing needs clinician oversight and approval | [12] | Adaptive dosing out of scope; the bolus calculator uses only doctor-set ratios, with a low-glucose block and maximum dose |
| LLMs hallucinate (~20% inappropriate answers) and change between versions | [14], [15] | Education-only scope, RAG over guidelines, repeatable safety test set |
| No evidence for LLM quality in Russian or Kazakh | [13]–[15] | Own bilingual test set before enabling the feature |
| Remote monitoring can cause over-involvement and conflict | [16], [17] | Opt-in, patient-controlled, scoped and revocable sharing |
| CGM data are re-identifiable; apps leak data to third parties | [9], [18] | Encryption, authentication, data minimisation, no third-party trackers, export and delete |
| Personal data of Kazakhstani users must be stored in Kazakhstan; transfer abroad needs a legal ground | [19] | Backend and database hosted in Kazakhstan; consent (parental for under-18s); no personal data sent to foreign services |

### Scope boundaries stated to the committee

- The application is a self-management support tool for education and risk awareness, not a certified medical device; it does not diagnose, and does not recommend or adjust insulin doses on its own. The meal bolus calculator applies the doctor’s prescribed ratio and is an academic prototype not intended for clinical use without certification.
- The ML model is validated retrospectively on CITY data; prospective clinical validation is outside the scope of a diploma project.
- Improvements in HbA1c or TIR cannot be claimed; the project measures prediction quality, usability and requirement fulfilment.

## References

[1] American Diabetes Association Professional Practice Committee, “6. Glycemic goals and hypoglycemia: Standards of Care in Diabetes—2025,” *Diabetes Care*, vol. 48, suppl. 1, pp. S128–S145, Jan. 2025, doi: 10.2337/dc25-S006.

[2] American Diabetes Association Professional Practice Committee, “14. Children and adolescents: Standards of Care in Diabetes—2025,” *Diabetes Care*, vol. 48, suppl. 1, pp. S283–S305, Jan. 2025, doi: 10.2337/dc25-S014.

[3] T. Battelino *et al.*, “Clinical targets for continuous glucose monitoring data interpretation: Recommendations from the international consensus on time in range,” *Diabetes Care*, vol. 42, no. 8, pp. 1593–1603, Aug. 2019, doi: 10.2337/dci19-0028.

[4] L. M. Laffel *et al.*, “Effect of continuous glucose monitoring on glycemic control in adolescents and young adults with type 1 diabetes: A randomized clinical trial,” *JAMA*, vol. 323, no. 23, pp. 2388–2396, Jun. 2020, doi: 10.1001/jama.2020.6940.

[5] C. Duckworth *et al.*, “Explainable machine learning for real-time hypoglycemia and hyperglycemia prediction and personalized control recommendations,” *J. Diabetes Sci. Technol.*, published online 2022, doi: 10.1177/19322968221103561.

[6] F. Prendin, J. Pavan, G. Cappon, S. Del Favero, G. Sparacino, and A. Facchinetti, “The importance of interpreting machine learning models for blood glucose prediction in diabetes: An analysis using SHAP,” *Sci. Rep.*, vol. 13, Art. no. 16865, 2023, doi: 10.1038/s41598-023-44155-x.

[7] D. N. Katsarou *et al.*, “Optimizing hypoglycaemia prediction in type 1 diabetes with ensemble machine learning modeling,” *BMC Med. Inform. Decis. Mak.*, vol. 25, Art. no. 46, 2025, doi: 10.1186/s12911-025-02867-2.

[8] J. Caterson, A. Lewin, and E. Williamson, “The application of explainable artificial intelligence (XAI) in electronic health record research: A scoping review,” *Digit. Health*, vol. 10, pp. 1–12, 2024, doi: 10.1177/20552076241272657.

[9] A. Geirhos *et al.*, “Standardized evaluation of the quality and persuasiveness of mobile health applications for diabetes management,” *Sci. Rep.*, vol. 12, Art. no. 3639, 2022, doi: 10.1038/s41598-022-07544-2.

[10] T. Cloarec, K. Cunneen, D. Nickson, S. Leigh, P. Hanson, and C. Toro, “Functions, features, and psychological well-being impacts of type 1 diabetes self-management mobile and web apps: Systematic review,” *J. Med. Internet Res.*, vol. 27, Art. no. e75280, 2025, doi: 10.2196/75280.

[11] L. Pi, X. Shi, Z. Wang, and Z. Zhou, “Effect of smartphone apps on glycemic control in young patients with type 1 diabetes: A meta-analysis,” *Front. Public Health*, vol. 11, Art. no. 1074946, Mar. 2023, doi: 10.3389/fpubh.2023.1074946.

[12] A. Kobayati *et al.*, “A Bayesian decision support system for automated insulin doses in adults with type 1 diabetes on multiple daily injections: A randomized controlled trial,” *Nat. Commun.*, vol. 16, Art. no. 8593, Sep. 2025, doi: 10.1038/s41467-025-63671-0.

[13] D. Wang *et al.*, “Enhancement of the performance of large language models in diabetes education through retrieval-augmented generation: Comparative study,” *J. Med. Internet Res.*, vol. 26, Art. no. e58041, 2024, doi: 10.2196/58041.

[14] Y. D. Ongen, A. İ. Aydın, M. Atak, and E. Eren, “Performance of several large language models when answering common patient questions about type 1 diabetes in children: Accuracy, comprehensibility and practicality,” *BMC Pediatr.*, vol. 25, Art. no. 799, 2025, doi: 10.1186/s12887-025-05945-6.

[15] C. T. Chang *et al.*, “Red teaming ChatGPT in medicine to yield real-world insights on model behavior,” *npj Digit. Med.*, vol. 8, Art. no. 149, 2025, doi: 10.1038/s41746-025-01542-0.

[16] E. R. Fremont and V. A. Miller, “Perspectives on remote glucose monitoring in youth with type 1 diabetes,” *J. Pediatr. Psychol.*, vol. 46, no. 10, pp. 1276–1285, Aug. 2021, doi: 10.1093/jpepsy/jsab068.

[17] W. H. Polonsky and A. L. Fortmann, “Impact of real-time CGM data sharing on quality of life in the caregivers of adults and children with type 1 diabetes,” *J. Diabetes Sci. Technol.*, vol. 16, no. 1, pp. 97–105, Jan. 2022, doi: 10.1177/1932296820978423.

[18] P. Dario, “Data privacy, ownership, and secondary use of clinical data generated by continuous glucose monitors and mobile health applications: A review,” *J. Diabetes Sci. Technol.*, published online 2026, doi: 10.1177/19322968261455365.

[19] *On Personal Data and Their Protection*, Law of the Republic of Kazakhstan No. 94-V, May 21, 2013 (as amended, 2026). [Online]. Available: [<u>https://adilet.zan.kz/rus/docs/Z1300000094</u>](https://adilet.zan.kz/rus/docs/Z1300000094)

[20] E. Cengiz et al., “ISPAD Clinical Practice Consensus Guidelines 2022: Insulin treatment in children and adolescents with diabetes,” *Pediatric Diabetes*, vol. 23, no. 8, pp. 1277–1296, Dec. 2022, doi: 10.1111/pedi.13442.

[21] S. Sampath, P. Tkachenko, E. Renard, and S. V. Pereverzev, “Glycemic control indices and their aggregation in the prediction of nocturnal hypoglycemia from intermittent blood glucose measurements,” J. Diabetes Sci. Technol., vol. 10, no. 6, pp. 1245–1250, 2016, doi: 10.1177/1932296816670400.

[22] S. Kodama et al., “Ability of current machine learning algorithms to predict and detect hypoglycemia in patients with diabetes mellitus: Meta-analysis,” JMIR Diabetes, vol. 6, no. 1, Art. no. e22458, 2021, doi: 10.2196/22458.
