# Existing Systems Comparison

No existing system combines manual-entry glucose monitoring, a parent mode with critical alerts, doctor access, patient-controlled consent and explainable risk assessment. This section compares seven systems against nine criteria to show this gap.

## 1. Selection of systems

We selected seven systems that represent the solutions a family with a child or adolescent with type 1 diabetes (T1D) can use today. They fall into three groups: logbook apps that work with a regular glucometer, CGM ecosystems that require the manufacturer’s sensor, and an open-source remote monitoring tool. LinX and Ottai were included as lower-cost CGM alternatives; Ottai’s App Store listing covers Russia, which makes it relevant to the CIS market. The interviewed patient (E2) uses Ottai and Libre sensors, which confirms that these systems are used in Kazakhstan.

| Group | System | Developer | Reason for selection |
| --- | --- | --- | --- |
| Logbook apps | mySugr | Roche | One of the most widely used logbook apps; supports manual entry |
| Logbook apps | Glooko | Glooko | Represents patient-to-clinic data sharing |
| CGM ecosystems | FreeStyle LibreLink + LibreLinkUp | Abbott | CGM app with a caregiver app; direct analogue of our parent mode |
| CGM ecosystems | Dexcom G7 + Dexcom Follow | Dexcom | CGM app with a caregiver app; includes a predictive alert |
| CGM ecosystems | LinX CGM | MicroTech Medical | Lower-cost CGM with share/follow |
| CGM ecosystems | Ottai + Ottai Share | Ottai | Lower-cost CGM that advertises AI-powered insights |
| Open source | Nightscout | Community | Self-hosted remote monitoring for caregivers |

Evidence rule. Every rating is based on an official source: a manufacturer website, an app store listing, a support page or official documentation. If an official source does not describe a feature, it is marked “not stated”. This means there is no public evidence for the feature, not proof that it does not exist.

## 2. Comparison criteria

The nine criteria were derived from the survey of 20 parents (E1), the patient interview (E2) and the general practitioner interview (E3), described in the Evidence of Relevance document, and are supported by the literature review. Each criterion corresponds to a need confirmed by at least one source (column “Evidence”).

| ID | Criterion | Why it matters for the project | Evidence |
| --- | --- | --- | --- |
| C1 | Works with a regular glucometer (manual entry, no CGM required) | Many families do not have a CGM; target users include meter-only patients | E1: 11 of 20 families have no CGM, 13 use a glucometer (F3); E2: glucometer 6–7×/day; E3: D1, D8 |
| C2 | Logging of food, carbohydrates and insulin | Needed to explain glucose changes | E2: paper diary abandoned, counts carbs manually (÷12); E3: D1 incomplete records; Lit. [6], [11] |
| C3 | Remote access for a parent or caregiver | Core need for children and adolescents with T1D | E1: only 4 of 20 parents see readings remotely (F2); Lit. [2], [16], [17] |
| C4 | Critical-value alerts delivered to the caregiver | Hypoglycemia safety when the child is not nearby | E1: 15 of 20 check glucose at night (F1), 16 of 20 want early warnings (F4); E2: severe night hypo (2.3 mmol/L); E3: D5 |
| C5 | Doctor access and reports | Doctors see only a few readings at visits | E3: doctor needs a short report (D2); E1: 9 of 20 want contact with the doctor; E2: visits every 3 months |
| C6 | Consent management: the patient grants and revokes access | Privacy and control over sensitive health data | E1: data security 5/5 for 17 of 20, child’s control 5/5 for 14 of 19 (F7); E3: D3; Lit. [16], [18], [19] |
| C7 | Risk prediction (predictive alerts or ML) | Early warning instead of reacting to an already critical value | E1: glucose forecast is the top feature, 15 of 20 (F5); Lit. [5] |
| C8 | Explanations and an educational AI assistant | Patients need to understand why glucose changed | E1: trust in AI is conditional (F8); E3: AI must explain its advice (D6), show link between habits and glucose (D4); Lit. [6], [14], [15] |
| C9 | Required hardware and cost | Accessibility for families | E1: sensors bought with own money, strip shortages (F3, F9), mixed willingness to pay (F10) |

## 3. Comparison results

Only the logbook apps work without a CGM, and none of them offer a parent mode. Every system with caregiver alerts requires a manufacturer’s sensor. Legend: ✔ supported; ✖ not supported; ⚠ partial or with conditions; - not stated in official sources.

| Criterion | mySugr | Glooko | Libre | Dexcom | LinX | Ottai | Nightscout | Proposed |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C1 Manual entry, no CGM | ✔[1] | ✔[2] | ✖ sensor [3] | ✖ sensor [6] | ✖ sensor [13] | ✖ sensor [15] | ⚠ built around CGM [10] | ✔ |
| C2 Food, carbs, insulin | ✔[1] | ✔[2] | ⚠ notes [5] | – | ⚠ BG only [14] | ✔ [17] | ✔ [10] | ✔ |
| C3 Caregiver access | – [1] | – [2] | ✔ up to 20 [3] | ✔ up to 10 [6][7] | ✔ up to 20 [14] | ✔ Ottai Share [16] | ✔ [9] | ✔ invite code |
| C4 Alerts to caregiver | ✖ own device [1] | – | ✔ [3] | ✔ [7] | – | – | ✔ [9] | ✔ |
| C5 Doctor access, reports | ⚠ paid PRO [1] | ✔ clinic code [2][12] | ⚠ LibreView [3] | – | ⚠ AGP export [14] | ⚠ via invitation [16][17] | ⚠ no doctor role [11] | ✔ dashboard |
| C6 Patient grants, revokes | – | ⚠ no revoke [2] | ✔[4] | ✔ [7][8] | – | ⚠ no revoke [16] | ⚠ admin tokens [11] | ✔ audit log |
| C7 Risk prediction | ⚠ one CGM, 3 countries [1] | – | – | ✔ 20 min ahead [6] | – | – | ⚠ AR2 forecast [9][10] | ✔ |
| C8 Explanations, AI assistant | ✖ [1] | ✖[2] | ✖ | ✖ | ✖ [13] | ⚠ “AI insights”, unexplained [17] | ✖ | ✔ safe AI coach |
| C9 Hardware, cost | Any meter; PRO $4.99/mo[1] | Free; 200+ devices [2] | Libre sensor [3] | Dexcom CGM [6] | LinX sensor [13] | Ottai sensor; lower price [15][18] | Free; self-hosted [9] | Any meter, phone |

## 4. Analysis

### 4.1 What existing systems do well

- Logging and reports are mature in mySugr and Glooko: manual entry, food databases, device sync, PDF reports and clinic sharing[1][2].

- Remote monitoring by family is well developed in CGM ecosystems. LibreLinkUp supports up to 20 followers and Dexcom Follow up to 10, with real-time caregiver alerts [3][7]. Lower-cost brands copy this model: LinX supports up to 20 followers [14] and Ottai uses a separate Ottai Share app [16].

- Predictive alerts exist for CGM data: Dexcom “Urgent Low Soon” warns up to 20 minutes ahead [6], and Nightscout offers AR2 forecast alarms [9].

### 4.2 Limitations and gaps

1. Caregiver monitoring is tied to CGM hardware. LibreLinkUp, Dexcom Follow, LinX and Ottai Share all require the same brand’s sensor [3][6][13][15]. Lower-cost brands do not remove this barrier, because the family still has to buy sensors regularly. The logbook apps that support manual entry do not describe caregiver sharing or caregiver alerts [1][2].

2. Prediction exists only for CGM data, and it is not explained. No system describes risk assessment for families who use manual readings. This matches the literature, where most hypoglycemia prediction models use CGM data. The existing predictive alerts do not explain which factors caused the warning.

3. No explanation of causes. No system describes an assistant that explains glucose patterns in plain language. Ottai advertises “AI powered insights” [17], but official sources do not describe what they explain or which safety limits apply. mySugr offers a bolus calculator [1], which shows that a calculator with user-set ratios is an accepted feature in diabetes apps. Our calculator follows the same principle: it applies only the doctor’s ratio and never adjusts doses on its own.

4. Consent is fragmented. Each vendor has its own sharing model: followers, invitations, clinic codes or admin tokens. None combines parent and doctor access with an audit log in one patient-controlled consent flow [11].

5. Open-source alternatives require technical skills. Nightscout must be self-hosted with Node.js and MongoDB, which is a barrier for most families.

## 5. Conclusion

No analysed system combines all five of the following:

1. Monitoring with a regular glucometer and manual entry.

2. A parent mode with critical alerts.

3. Doctor access with weekly reports.

4. Patient-controlled consent with an audit log.

5. Risk prediction that works with manual readings (an explainable ML model evaluated on the CITY dataset) and a safe educational assistant.

The proposed system fills this gap for families of children and adolescents with T1D who do not use a CGM, or who want one place for logs, family support and the doctor.

Note on the ML component. Manual readings (4–7 per day) are too sparse for short-term prediction within minutes. The model therefore predicts nocturnal hypoglycemia at bedtime, over the next 8 hours. It is evaluated on the CITY dataset: labels come from dense CGM data, and model inputs use only simulated meter readings.

The proposed system does not compete with CGM devices. It does not replace device alerts, does not recommend or adjust insulin doses on its own (the meal bolus calculator applies only the doctor’s ratio) and remains a research prototype.

## 6. Limitations of the comparison

- Ratings are based on public official sources only. Features that are not publicly described could not be verified without using the products.

- Features and availability differ by country and app version. Availability in Kazakhstan was confirmed only for Libre and Ottai (E2).

- Locally developed applications (KZ, CIS) were not included. If interviewers name local tools, they should be added.

- For LinX, the most detailed source is the manufacturer’s FAQ document hosted by a distributor [14]. Ottai’s AI claims come from the company’s own website and press materials [17][18], not from an independent evaluation.

## References

All sources accessed 19–21 October 2026.

1. Roche. mySugr – Diabetes Tracker Log, Apple App Store listing. <https://apps.apple.com/app/id516509211>

2. Glooko. Glooko – Track Diabetes Data, Apple App Store listing. <https://apps.apple.com/app/id471942748>

3. Abbott. FreeStyle LibreLinkUp. <https://pro.freestyle.abbott/ie-en/home/freestyle-portfolio/digital-health-solutions/freestyle-librelinkup.html>

4. Abbott. Can a FreeStyle LibreLink app user turn off sharing after they have invited someone? <https://www.freestyle.abbott/en-ae/support/can-a-freestyle-librelink-app-user-turn-off-sharing-after-they-have-invited-someone-to-see-their-glucose-data-in-the-librelinkup-app-.html>

5. Abbott. Adding food, insulin and exercise notes in FreeStyle LibreLink. <https://www.freestyle.abbott/en-us/support/faq/apps/adding-food-insulin-and-exercise-notes-in-freestyle-librelink.html>

6. Dexcom. Dexcom G7 CGM System – Type 1. <https://www.dexcom.com/en-us/g7-cgm-system/type-1>

7. Dexcom. Dexcom Follow app. <https://www.dexcom.com/en-CA/apps/dexcom-follow>

8. Dexcom. Sharing with Followers (FAQ). <https://www.dexcom.com/en-us/faqs/follow/sharing-with-followers>

9. Nightscout. cgm-remote-monitor README, GitHub. <https://github.com/nightscout/cgm-remote-monitor>

10. Nightscout documentation. Discover your Nightscout site. <https://nightscout.readthedocs.io/en/latest/nightscout/discover.html>

11. Nightscout documentation. Admin Tools. <https://nightscout.readthedocs.io/en/latest/nightscout/admin_tools.html>

12. Glooko. Sharing your Glooko Data. <https://support.glooko.com/hc/en-us/sections/360000451758-Sharing-your-Glooko-Data>

13. Micro Tech Medical (Hangzhou). LinX CGM, Apple App Store listing. <https://apps.apple.com/mr/app/linx-cgm/id6466264731>

14. MicroTech Medical. LinX FAQs (manufacturer document, distributor copy). <https://zue.hu/shop_ordered/73600/pic/Linx_FAQs.pdf>

15. Ottai Technology (Wuxi). Ottai, Apple App Store listing. <https://apps.apple.com/ph/app/ottai/id6476258890>

16. Ottai. Ottai Share, Google Play listing. <https://play.google.com/store/apps/details?id=com.ottai.share&hl=en>

17. Ottai. Ottai App (official website). <https://www.ottai.com/en-PH/app>

18. FutureFive. Ottai CGM tops 1 million users in global expansion. <https://futurefive.com.au/story/ottai-cgm-tops-1-million-users-in-global-expansion>
