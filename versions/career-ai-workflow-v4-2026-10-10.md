---
name: career-ai-workflow-v4-2026-10-10
description: Version snapshot of the career-ai agent system. v4 pairs the agent playbook with the JobPulse app, which runs the same playbook as code and adds a gated auto-submit path. Source of truth stays live at /workflows/career-ai/career-acceleration-playbook.md; this file is a point-in-time record, not maintained going forward.
version: 4
snapshot_date: 2026-10-10
supersedes: v3 (Resume Intelligence Layer)
---

# Career-AI Workflow — v4 Snapshot (2026-10-10)

## What's new in this version

The agent playbook and the JobPulse app ([AI-JOB-LEAD-AND-SUBMISSION](https://github.com/NickyMontederamos/AI-JOB-LEAD-AND-SUBMISSION))
were two separate ideas. They are now one flow: the app runs this playbook as code
behind a step-by-step UI (Profile → Discover → Rank → Tailor → Submit → Track).

| # | Change | Why |
|---|---|---|
| 1 | **The app ports the playbook's rules unchanged:** matching rubric (50/20/15/10/5), `application_strategy` labels, ATS alignment heuristic (60/25/15, target 75, max 2 revisions, no fabrication), role signals and the applications-log schema | One set of rules, whether a run happens in Claude Code or in the app |
| 2 | **Stage 5 now has two paths:** manual (default) or the app's gated auto-submit | Nicole chose fully automatic submission. The gate keeps it to what is safe and permitted. |
| 3 | **Auto-submit gate:** `apply_now` only; ATS ≥ 75 with format compliance passing and zero fabrication flags; complete contact info; daily cap; no duplicates; email (her Gmail, send-only) or Greenhouse/Lever/Ashby forms only | Stops weak or invented drafts from going out without review, and keeps her accounts safe |
| 4 | **Never automated:** LinkedIn, Indeed, Jobstreet, Glassdoor, Kalibrr, OnlineJobs.ph, Upwork, and any CAPTCHA form | Their terms prohibit bots, and bypassing a CAPTCHA risks account bans |
| 5 | **EVALS Scenario 4 rewritten; Scenarios 18-20 added** for the gate, unknown form questions and unconfirmed submits | The old Scenario 4 assumed nothing could ever submit |
| 6 | **The app's Track step exports `applications-log.md`** in this repo's exact column schema | Outcomes flow back into `/outputs/career-ai/` for career-matching-agent's history component |

## What did not change

- No agent in `.claude/agents/` submits anything. They prepare materials only.
- Stages 0-4 and Stage 6 are unchanged.
- The archetype files in `/resources/career-ai/resume-profiles/` remain agent-side
  only. The app builds strategy directly from the profile, which is the playbook's own
  "none — built from profile" fallback.
