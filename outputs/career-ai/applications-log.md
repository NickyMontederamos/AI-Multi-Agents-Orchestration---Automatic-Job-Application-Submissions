---
name: applications-log
description: One row per application. Read by career-matching-agent to spot response patterns over time (the system's "continuous learning" mechanism), and by resume-writer-agent's resume_version references. Append rows directly when Nicole reports an outcome — no dedicated agent needed for this.
---

# Applications Log

| Date | Role | Company | Source | Match Score (at time) | application_strategy | Status | Outcome | ats_score | resume_version | market_signal | job_board | response_days | interview_rounds | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | | | | | |

**Status values:** applied / interviewing / rejected / no-response / offer / withdrawn.

**Outcome values (for future analytics):** No response | Recruiter reply | Assessment
| Interview | Offer.

**Column notes:**
- `application_strategy` — the label career-matching-agent assigned at the time
  (apply_now / stretch_role / networking_target / avoid), so later analysis can check
  whether the classification actually predicted anything.
- `ats_score` — resume-ats-scorer-agent's final ats_alignment_heuristic total for the
  version actually submitted (a heuristic estimate, never treated as a cause of the
  outcome by itself).
- `resume_version` — the exact version identifier from resume-writer-agent
  (e.g. `acme-secops-v2`), so a specific draft can be traced to a specific outcome.
- `market_signal` — the role-signals.md status at time of application (in-demand /
  rising / declining / unlisted).
- `job_board` — where it was found (Indeed, Jobstreet PH, OnlineJobs.ph, referral,
  etc.).
- `response_days` — calendar days between applying and first response, when known.
- `interview_rounds` — count, when applicable.

Once this table has real volume (dozens of rows spanning a mix of outcomes), it
becomes worth exploring an actual trained matching model fed by this data — per
career-ai v3's review, this is deliberately deferred until there's real signal to
train on. Until then, career-matching-agent reasons over it directly and explains
what it sees, and these richer columns exist so that future analysis (e.g. "did
higher ats_score actually correlate with interviews?") has something to work with
without needing to re-derive it after the fact.
