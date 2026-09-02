---
name: ranked-matches-2026-09-02
description: Scored and ranked shortlist from postings-2026-09-02.md, matched against candidate-profile.md and role-signals.md. Written by career-matching-agent.
inputs:
  profile: /resources/career-ai/candidate-profile.md (profile_status: current, last_updated 2026-09-02)
  role_signals: /resources/career-ai/role-signals.md (last_refreshed 2026-09-02 — current, no staleness flag needed this run)
  postings_batch: /drafts/career-ai/postings-2026-09-02.md (21 scored postings; 3 senior/principal-level postings excluded from scoring by the research agent as far outside profile scope — carried forward below, not scored)
  applications_log: /outputs/career-ai/applications-log.md (header row only, no data — historical-outcomes component contributes 0 to every score below; no pattern exists yet to surface)
scoring_weights: "skills 50% / experience 20% / role-signal 15% / location-remote 10% / historical-outcomes 5%"
---

# Ranked Matches — 2026-09-02

Scored all 21 reportable postings from the batch — nothing pre-filtered before scoring, per rubric. Historical-outcomes component is 0 for every posting this run because `applications-log.md` has no rows yet; that's not a penalty, it's an honest "no data" — flagged once here rather than repeated 21 times.

Score format: weighted total /100, followed by each component's **point contribution** to that total (not raw sub-scores), so it's visible which factors pulled the number up or down.

---

## Tier 1 — Strongest Matches

### 1. Junior ServiceNow Developer — General Dynamics Information Technology — **66.0/100**
Skills +27.5 | Experience +15 | Role-signal +13.5 | Location +10 | History +0
**In-demand** (ServiceNow Developer/Administrator, ~₱567K/yr avg category)
Explicitly entry-level with a "CS background, system design fundamentals" bar she clears via CSA + hands-on Flow Designer/Script Includes/Catalog work, and it's the only posting in the batch where the stated experience level (junior) actually matches her real ~4-months-hands-on-ServiceNow / 3-years-total-IT profile instead of requiring her to stretch — the best experience-overlap fit in the whole batch.

### 2. ServiceNow (Multiple Roles) — Sky Solutions LLC — **59.0/100**
Skills +27.5 | Experience +8 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
"Active CSA certification mandatory for all roles" is a bar she clears directly and confidently — the strongest single skills-match signal in the batch — but the posting is vague on module specialization and seniority, so experience overlap is scored conservatively rather than assumed favorable.

---

## Tier 2 — Solid Matches, Worth a Look

### 3. ServiceNow Developer — Teknita LLC — **49.0/100**
Skills +22.5 | Experience +3 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
Explicitly wants Flow Designer proficiency — she has hands-on Flow Designer experience (the "Incident Priority Handler" flow), a genuine specific skill hit — but the posting wants 4+ years of ServiceNow development against her ~4 months, which is a real gap the score reflects honestly rather than smoothing over.

### 4. Sales Intern (ServiceNow/Agentic AI interest) — Work4Flow — **48.75/100**
Skills +20 | Experience +6 | Role-signal +12.75 | Location +10 | History +0
**Rising** (Agentic AI Engineer / AI Integration Engineer — matches her CV's stated Agentic AI/Generative AI specialization)
Directionally interesting because it's the only posting that explicitly wants ServiceNow-and-Agentic-AI interest, which lines up with her stated specialization, but it's a sales role, not an engineering one — worth a second look as a low-barrier foot-in-the-door, not as a technical-track match, and that mismatch is why it doesn't score higher despite the favorable rising-role signal.

### 5. ServiceNow Developer/Administrator — 9th Way Insignia — **48.0/100**
Skills +17.5 | Experience +7 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
General ITSM/Agile alignment from her bootcamp exposure, but ITOM and CMDB platform support aren't evidenced anywhere in her profile, and the posting states no years requirement to calibrate experience overlap against, so both components are scored on partial, generic alignment rather than a confirmed strong fit.

### 6. ServiceNow Developer — Oscar Technology — **47.0/100**
Skills +20 | Experience +5 | Role-signal +13.5 | Location +8.5 | History +0
**In-demand**
CSA certification matches directly, but "financial-services implementation experience" is a specific domain requirement she doesn't have evidence for, and the role includes occasional Pittsburgh travel, which is a small but real deduction against her general remote preference.

### 7. Mid-Level ServiceNow Developer — Blu Omega — **47.0/100**
Skills +17.5 | Experience +6 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
Wants 3 years of experience specifically — she has 3 years of *total* IT experience but only ~4 months of it is ServiceNow, so this is scored as a real gap rather than treated as a met requirement just because the topline number happens to match; flagging that distinction explicitly since it's the kind of thing that's easy to over-credit.

### 8. ServiceNow Vulnerability Response Engineer — SIXGEN — **46.5/100**
Skills +20 | Experience +3 | Role-signal +13.5 | Location +10 | History +0
**In-demand / rising mix** (Cybersecurity/Security Engineer = in-demand; Cloud Security Engineer, AI Governance/GRC Analyst = rising)
**Notable stretch role worth a second look**: this is the closest thing in the batch to her *stated top-priority* goal (Security Engineer/DevSecOps via ServiceNow Security modules), and her CSA plus general security background (Splunk home lab, vuln management/threat analysis fundamentals, TryHackMe Hacker rank) gives it real partial skills credit — but the posting wants 5+ years of enterprise vulnerability management and hands-on ServiceNow SecOps/VR/SIR/GRC modules specifically, which she doesn't have yet, so the experience component is scored honestly low despite the strong directional fit. Worth tracking as a target-profile role to build toward, not necessarily to apply to today.

### 9. ServiceNow Developer — Star Seven Six — **45.5/100**
Skills +15 | Experience +7 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
Requirements are vague ("CS background, research capabilities") with no explicit skill list or years stated, so both skills and experience are scored on generic, unconfirmed alignment rather than anything specific she can point to.

### 10. ServiceNow Developer — Work4Flow — **45.0/100**
Skills +17.5 | Experience +4 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
Wants 3–5 years plus JavaScript and API integration depth that isn't clearly evidenced in her profile beyond Script Includes work; platform-configuration exposure is a real partial match, but the years gap is significant.

### 11. ServiceNow Mid Business Analyst — Bravium Consulting — **44.5/100**
Skills +15 | Experience +6 | Role-signal +13.5 | Location +10 | History +0
**In-demand** (Business Analyst category)
No explicit ServiceNow-specific skill list beyond the title and a "CS background" note, so this scores on generic mid-level alignment; she has no formal BA-titled experience, which keeps skills and experience both moderate rather than strong.

---

## Tier 3 — Weaker Fit or Real Constraints

### 12. Remote ServiceNow Administrator — Apex Systems — **42.5/100**
Skills +12.5 | Experience +7 | Role-signal +13.5 | Location +9.5 | History +0
**In-demand**
ITOM software exposure is required and isn't in her profile; data-standards-compliance language is generic enough that it's hard to credit specific overlap.

### 13. ServiceNow Admin :: Remote — Arkhya Tech — **41.5/100**
Skills +17.5 | Experience +2 | Role-signal +13.5 | Location +8.5 | History +0
**In-demand**
CSA matches, and Now Assist configuration experience is at least directionally close to her Agentic AI specialization interest, but it wants 5–7 years hands-on ServiceNow plus 2 additional CIS certifications she doesn't hold — a large experience-overlap gap the score reflects directly rather than letting the cert alignment paper over it.

### 14. Security GRC Analyst — Protective — **41.25/100**
Skills +12.5 | Experience +6 | Role-signal +12.75 | Location +10 | History +0
**Rising** (AI Governance/GRC Analyst)
General GRC, not ServiceNow-module-specific, so her ServiceNow platform experience doesn't transfer directly here; Azure/AWS knowledge is required and isn't evidenced in her profile, though her risk-management/risk-assessment functional skills give partial credit.

### 15. ServiceNow Senior Technical Consultant — Code Repo — **39.0/100**
Skills +12.5 | Experience +3 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
Requires CSA *and* CAD certifications together (she holds CSA, CAD is still in progress) plus technical-lead experience she doesn't have yet — the "Senior" framing and lead-role expectation are the main drag on both skills and experience components.

### 16. ServiceNow Business Analyst — eCIFM Solutions Inc. — **39.0/100**
Skills +12.5 | Experience +3 | Role-signal +13.5 | Location +10 | History +0
**In-demand** (Business Analyst)
Wants 3–5 years of BA-titled experience plus 2+ years on the ServiceNow platform specifically and ITIL knowledge not evidenced in her profile; she has some Agile-adjacent project exposure but no formal BA title history, so this is a genuine stretch rather than a close fit.

### 17. Senior ServiceNow Developer III — CorVel Corporation — **38.0/100**
Skills +12.5 | Experience +2 | Role-signal +13.5 | Location +10 | History +0
**In-demand**
"Senior... III" with 5 years required is a significant seniority gap against her ~4 months hands-on ServiceNow experience; no specific skill list beyond the years requirement to credit against.

### 18. ServiceNow Business Process Analyst — DirectViz Solutions, LLC — **35.5/100**
Skills +15 | Experience +6 | Role-signal +13.5 | Location +1 | History +0
**In-demand** (Business Analyst)
**Location flag**: labeled "remote" but the posting requires the candidate be local to the DC area — a real constraint that conflicts with her general-remote (not US-metro-restricted) preference, which is why location scores near zero here despite the "remote" label.

### 19. ServiceNow GRC Developer — Nobl Q — **34.75/100**
Skills +10 | Experience +2 | Role-signal +12.75 | Location +10 | History +0
**In-demand / rising mix**
Wants 5 years of experience and JavaScript proficiency at a depth not clearly evidenced in her profile; GRC-module hands-on experience (like the SIXGEN posting above) is aspirational for her, not yet demonstrated.

### 20. Jr. Systems Administrator — Paragone Solutions, Inc. — **29.0/100**
Skills +15 | Experience +10 | Role-signal +3 | Location +1 | History +0
**Signal flagged as ambiguous, leaning declining** — role-signals.md splits "Systems Administrator" into an in-demand cloud-oversight half and a declining "Traditional (Manual) System Administrator" half; this posting's brief ("analysis skills," part-time, no cloud-platform specifics) reads closer to the traditional/manual half, so it's scored on the declining end rather than defaulted to the in-demand table entry.
**Location flag**: explicitly requires US work authorization, which she does not have as a PH-based candidate — the main driver of the low score alongside the role-signal lean. Flagging this instead of silently dropping it, per the "never drop without saying why" rule.

### 21. ServiceNow Business Analyst — Accenture Federal Services — **27.0/100**
Skills +10 | Experience +3 | Role-signal +13.5 | Location +0.5 | History +0
**In-demand** (Business Analyst)
Lowest score in the batch. Despite being an Accenture entity, this is Accenture's US federal-contracting arm — distinct from Accenture Philippines — and explicitly requires US work authorization she doesn't have; that single constraint dominates the location component and drags the total down even though role-signal and general BA demand are favorable.

---

## Excluded from Scoring (carried forward, not dropped silently)

The research agent pulled these 3 postings out before handoff as far outside profile scope (11–14+ years or principal/architect-level, vs. her ~3 years total IT / ~4 months ServiceNow) and did not include them in the scored batch. Per that same call, they're not re-scored here — scoring them would produce a near-zero experience component that adds no decision value — but they're listed so nothing is silently missing:
- ServiceNow GRC/IRM Business Process Consultant — LTM (11+ yrs)
- ServiceNow GRC Architect (Senior) — Scalable Systems Inc (11+ yrs)
- ServiceNow Architect (CTA)/Lead Engineer/Dev Manager/PM roles — Work4Flow (5–8+ yrs)
- ServiceNow Business Process Consultant (Manager) — Huron Consulting
- Principal Advisory Consultant — CDW

## Gaps and Notes on This Run
- **Historical-outcomes component (5% weight) contributed 0 to every score.** `applications-log.md` has no data rows yet — nothing to pattern-match against. Once a handful of real outcomes are logged, re-run matching to let this component start doing real work.
- **role-signals.md is current** (last refreshed today, 2026-09-02) — no staleness flag needed this run, but note for future runs six months out.
- **Profile is current and complete enough to score confidently** — no missing preference fields forced a scoring gap this run.
- Several postings state "DOE" or no pay figure; per the rubric, pay was never a scoring input regardless, so this had no effect on any score.
