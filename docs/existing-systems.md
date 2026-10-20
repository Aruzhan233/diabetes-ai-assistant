# Existing Systems Comparison

**Project:** Development of a Mobile Application for Diabetes Monitoring and Management Using Artificial Intelligence Methods (Diabetes AI Assistant)
**Tasks:** DIP-10 (criteria, systems 1–2), DIP-11 (systems 3–7, conclusion)
**Sources accessed:** 19–21 October 2026

## 1. Purpose and method

This document compares existing diabetes management systems against a single, consistent set of criteria. The goal is to show which needs are already covered and which gap the proposed solution addresses.

**System selection.** We selected seven systems that represent the main types of solutions a family with a child with type 1 diabetes (T1D) can use today:

| # | System | Type | Why it was selected |
|---|---|---|---|
| 1 | **mySugr** (Roche) | Self-management logbook app | One of the most widely used logbook apps; supports manual entry |
| 2 | **Glooko** | Data platform for patients and clinics | Representative of patient-to-clinic data sharing |
| 3 | **FreeStyle LibreLink + LibreLinkUp** (Abbott) | CGM app + caregiver app | Direct analogue of our parent mode (remote following) |
| 4 | **Dexcom G7 + Dexcom Follow** (Dexcom) | CGM app + caregiver app | Direct analogue of our parent mode; includes a predictive alert |
| 5 | **Nightscout** | Open-source remote monitoring | Community solution for remote caregiver monitoring |
| 6 | **LinX CGM** (MicroTech Medical) | Lower-cost CGM app with share/follow | Lower-cost alternative to Abbott and Dexcom; has its own sharing ecosystem |
| 7 | **Ottai + Ottai Share** (Ottai) | Lower-cost CGM app + follower app | Lower-cost CGM that advertises "AI powered insights"; App Store listing includes Russia (CIS market) |

**Evidence rule.** Every rating is based on an official source: a manufacturer website, an app store listing, a support page, or official documentation (Section 5). If an official source does not describe a feature, the feature is marked "not stated". This means there is no public evidence for it, **not** proof that the feature does not exist.

**Legend:** ✅ supported · ⚠️ partially or with conditions · ❌ not supported · — not stated in official sources

## 2. Criteria

| ID | Criterion | Why it matters for the project |
|---|---|---|
| C1 | Works with a regular glucometer (manual entry, no CGM required) | Many families do not have a CGM; the target users include meter-only patients |
| C2 | Logging of food/carbohydrates and insulin | Needed to explain glucose changes and as model features |
| C3 | Remote access for a parent/caregiver | Core need for children and adolescents with T1D |
| C4 | Critical-value alerts delivered to the caregiver | Hypoglycemia safety when the child is not nearby |
| C5 | Doctor access and reports | Doctors see only a few readings at visits |
| C6 | Consent management: the patient grants and revokes access | Privacy and control over sensitive health data |
| C7 | Risk prediction (predictive alerts or ML) | Early warning instead of reacting to a value that is already critical |
| C8 | Explanations and an educational AI assistant (chat) | Patients need to understand *why* glucose changed |
| C9 | Required hardware and cost | Accessibility for families |

## 3. Comparison table

| Criterion | mySugr | Glooko | LibreLink + LibreLinkUp | Dexcom G7 + Follow | Nightscout | LinX CGM | Ottai + Ottai Share | **Proposed system** |
|---|---|---|---|---|---|---|---|---|
| **C1** Regular meter, manual entry | ✅ Manual entries supported [S1] | ✅ Data can be "manually inputted" [S2] | ❌ Requires a Libre 2 / 2 Plus sensor [S3] | ❌ Share feature comes with Dexcom CGM systems [S6] | ⚠️ "BG Check" entry exists, but the system is built around CGM [S10] | ❌ Requires a compatible LinX CGM sensor [S13] | ❌ Requires an Ottai sensor [S15] | ✅ Manual entry is the primary input |
| **C2** Food/carbs and insulin | ✅ Diet, carbs, medication [S1] | ✅ Food (barcode, search), insulin, medications [S2] | ⚠️ Notes for meals, insulin and workouts can be attached to readings [S5] | — | ✅ Carb and bolus entries in Careportal [S10] | ⚠️ Fingerstick BG can be recorded on the curve [S14]; food and insulin not stated | ✅ "Log meals, medications, sports, insulin" [S17] | ✅ |
| **C3** Parent/caregiver remote access | — No sharing feature described [S1] | — Family sharing not described [S2] | ✅ Up to 20 followers [S3] | ✅ Up to 10 followers [S6][S7] | ✅ Multiple caregivers view data in real time [S9] | ✅ Share/Follow, up to 20 sharers/followers [S14] | ✅ Followers view data in Ottai Share, by invitation [S16][S17] | ✅ Parent mode via invite code |
| **C4** Critical alerts to the caregiver | ❌ Alarms only on the user's own device [S1] | — | ✅ Followers set their own alarms, real-time notifications [S3] | ✅ Followers get high/low alerts [S7] | ✅ High/low alarms visible to any watcher [S9] | — Alerts described for the user only [S13] | — Alerts described for the user only [S17] | ✅ Alert to patient and linked parent |
| **C5** Doctor access and reports | ⚠️ PDF/Excel reports in paid PRO [S1] | ✅ Sharing with clinic via ProConnect code; reports [S2][S12] | ⚠️ Sharing requires LibreView registration [S3] | — | ⚠️ Reports module, no dedicated doctor role [S10][S11] | ⚠️ AGP report export [S14]; no doctor platform stated | ⚠️ Downloadable reports; healthcare professionals can follow via invitation [S16][S17] | ✅ Doctor dashboard and weekly report |
| **C6** Patient grants and revokes access | — (no sharing) | ⚠️ Parents/guardians approve sharing for minors; revoking not described [S2] | ✅ User can "stop sharing glucose data at any time" [S4] | ✅ User chooses what data/alerts followers get; can cancel sharing [S7][S8] | ⚠️ Access tokens with roles; revoked by the site admin, not a patient-facing consent flow [S11] | — Sharing set in app settings; revoking not described [S14] | ⚠️ Access by invitation; revoking not described [S16] | ✅ Invite code, revoke, consent records, audit log |
| **C7** Risk prediction | ⚠️ "Real-time predictions" only with Accu-Chek SmartGuide CGM, only in Lithuania, Kuwait and Qatar [S1] | — | — | ✅ "Urgent Low Soon" warns up to 20 min ahead (CGM-based) [S6] | ⚠️ AR2 forecast alarms (simple statistical forecast, CGM-based) [S9][S10] | — Only high/low threshold reminders [S13] | — Forecasting not described | ⚠️ MVP: rule-based risk alerts for manual readings. Research: explainable ML model predicting hypoglycemia 30–60 min ahead, trained on CGM data (CITY dataset) and compared with a rule baseline |
| **C8** Explanations / AI assistant | ❌ Not described; offers a bolus calculator (dose advice) in select countries [S1] | ❌ Not described [S2] | ❌ Not described | ❌ Not described | ❌ Not described | ❌ Not described [S13] | ⚠️ "AI powered insights" advertised [S17]; content and safety limits not described | ✅ AI coach with safety guardrails (no doses, no diagnosis) |
| **C9** Hardware and cost | Any meter (manual); basic free, PRO $4.99/month or $39.99/year (US) [S1] | Free app; syncs 200+ devices [S2] | Libre sensor required; LibreLinkUp is free [S3] | Dexcom CGM required; internet required [S6] | Free, open source (AGPL); self-hosting with Node.js and MongoDB [S9] | LinX sensor required; app free [S13] | Ottai sensor required; app free [S15]; positioned as lower-priced CGM [S18] | Any meter; smartphone only |

## 4. Analysis

### 4.1 What existing systems already do well
- **Logging and reports** (mySugr, Glooko) are mature: manual entry, food databases, device sync, PDF reports and clinic sharing.
- **Remote monitoring by family** is well developed in CGM ecosystems: LibreLinkUp (up to 20 followers) and Dexcom Follow (up to 10) provide real-time caregiver alerts. The patient controls the sharing. Lower-cost CGM brands copy this model: LinX (up to 20 followers) and Ottai (separate Ottai Share app).
- **Predictive alerts exist**, but only on continuous CGM data: Dexcom "Urgent Low Soon" (up to 20 minutes ahead) and Nightscout AR2.

### 4.2 Limitations and gaps
1. **Caregiver monitoring is tied to CGM hardware.** LibreLinkUp, Dexcom Follow, LinX and Ottai Share all require the same brand's sensor [S3][S6][S13][S15]. Even the lower-cost CGM brands do not remove this barrier: the family still has to buy sensors regularly. The logbook apps that support manual entry (mySugr, Glooko) do not describe any caregiver sharing or caregiver alerts [S1][S2]. A family that uses a regular glucometer has no parent mode.
2. **Prediction exists only for CGM data, and it is not explained.** None of the systems describes risk assessment for families who use manual readings. This matches the literature, where most hypoglycemia prediction models use CGM data (see `docs/literature-review.md`). The CGM-based predictive alerts that exist (Dexcom "Urgent Low Soon", Nightscout AR2) do not explain which factors caused the warning.
3. **No explanation of the causes.** None of the systems describes an assistant that explains glucose patterns in plain language. Ottai advertises "AI powered insights" [S17], but official sources do not describe what the insights explain, which data they use or which safety limits apply. mySugr instead offers a bolus calculator, which gives insulin dose advice — the opposite of our safety boundary.
4. **Consent is fragmented.** Each vendor has its own sharing model (followers, invitations, clinic codes, admin tokens). None combines parent and doctor access with an audit log in a single, patient-controlled consent flow. Nightscout access control requires technical administration [S11].
5. **Open-source alternatives require technical skills.** Nightscout must be self-hosted (Node.js, MongoDB) [S9], which is a barrier for most families.

### 4.3 Conclusion: why the proposed solution is needed
No single analysed system combines:
**(1)** manual-entry monitoring with a regular glucometer,
**(2)** parent mode with critical alerts,
**(3)** doctor access with weekly reports,
**(4)** patient-controlled consent with an audit log, and
**(5)** risk assessment that works with manual readings (rule-based in the MVP), an explainable ML prediction model evaluated on CGM data, and a safe educational assistant.

The proposed system fills this gap for families of children and adolescents with T1D who do not use a CGM, or who want one place for logs, family support and the doctor.

**Note on the ML component.** Manual readings (4–7 per day) are too sparse for reliable short-term prediction. The MVP therefore uses rule-based alerts for manual readings. The ML model is developed and evaluated offline on CGM data and can be used in the app only after CGM integration, which is planned as future work.

**Our system does not compete with CGM devices.** It does not replace device alerts, it does not calculate insulin doses, and it remains a research prototype.

## 5. Limitations of this comparison
- Ratings are based on public official sources only. Features that are not publicly described are marked "not stated" and could not be verified without using the products.
- Features and availability differ by country and app version. Availability in Kazakhstan was not verified and should be confirmed in local app stores and in the user interviews.
- Locally developed applications (Kazakhstan, CIS) were not included. If interviewees name local tools, they should be added.
- For LinX, the most detailed source is the manufacturer's FAQ document hosted by a distributor [S14]; no LinX caregiver or doctor portal is described on official pages. Ottai's AI claims come from the company's own website and press materials [S17][S18], not from an independent evaluation.

## 6. Sources (accessed 19–21 October 2026)

- **[S1]** mySugr – Diabetes Tracker Log, Apple App Store listing. https://apps.apple.com/app/id516509211
- **[S2]** Glooko – Track Diabetes Data, Apple App Store listing. https://apps.apple.com/app/id471942748
- **[S3]** Abbott. FreeStyle LibreLinkUp (healthcare professional page). https://pro.freestyle.abbott/ie-en/home/freestyle-portfolio/digital-health-solutions/freestyle-librelinkup.html
- **[S4]** Abbott support. Can a FreeStyle LibreLink app user turn off sharing after they have invited someone? https://www.freestyle.abbott/en-ae/support/can-a-freestyle-librelink-app-user-turn-off-sharing-after-they-have-invited-someone-to-see-their-glucose-data-in-the-librelinkup-app-.html
- **[S5]** Abbott support. Adding food, insulin and exercise notes in FreeStyle LibreLink. https://www.freestyle.abbott/en-us/support/faq/apps/adding-food-insulin-and-exercise-notes-in-freestyle-librelink.html
- **[S6]** Dexcom. Dexcom G7 CGM System – Type 1. https://www.dexcom.com/en-us/g7-cgm-system/type-1
- **[S7]** Dexcom. Dexcom Follow app. https://www.dexcom.com/en-CA/apps/dexcom-follow
- **[S8]** Dexcom FAQ. Sharing with Followers ("Removed" status). https://www.dexcom.com/en-us/faqs/follow/sharing-with-followers
- **[S9]** Nightscout cgm-remote-monitor README (GitHub). https://github.com/nightscout/cgm-remote-monitor
- **[S10]** Nightscout documentation. Discover your Nightscout site. https://nightscout.readthedocs.io/en/latest/nightscout/discover.html
- **[S11]** Nightscout documentation. Admin Tools (roles and tokens). https://nightscout.readthedocs.io/en/latest/nightscout/admin_tools.html
- **[S12]** Glooko support. Sharing your Glooko Data. https://support.glooko.com/hc/en-us/sections/360000451758-Sharing-your-Glooko-Data
- **[S13]** LinX CGM, Apple App Store listing (Micro Tech Medical (Hangzhou) Co., Ltd.). https://apps.apple.com/mr/app/linx-cgm/id6466264731
- **[S14]** MicroTech Medical. LinX FAQs (manufacturer document, distributor copy). https://zue.hu/shop_ordered/73600/pic/Linx_FAQs.pdf
- **[S15]** Ottai, Apple App Store listing (Ottai Technology (Wuxi) Co., Ltd.). https://apps.apple.com/ph/app/ottai/id6476258890
- **[S16]** Ottai Share, Google Play listing. https://play.google.com/store/apps/details?id=com.ottai.share&hl=en
- **[S17]** Ottai. Ottai App (official website). https://www.ottai.com/en-PH/app
- **[S18]** FutureFive. Ottai CGM tops 1 million users in global expansion. https://futurefive.com.au/story/ottai-cgm-tops-1-million-users-in-global-expansion
