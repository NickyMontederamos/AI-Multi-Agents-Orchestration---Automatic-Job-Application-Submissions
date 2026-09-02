---
name: career-acceleration-playbook
description: Orchestration spec for the career-ai agent system (v3 — Resume Intelligence Layer). Not an agent itself — this is the playbook the main Claude session follows to dispatch career-profile-agent, career-research-agent, career-matching-agent, resume-strategy-agent, resume-writer-agent, and resume-ats-scorer-agent in the right order, with the right handoffs and gates.
---

# Career Acceleration Playbook (v3)

## Why there's no separate "orchestrator" agent

In Claude Code, the main session you're talking to already plays that role — it reads
each specialist agent's `description` and dispatches accordingly via the Agent tool.
A dedicated orchestrator subagent whose only job is "call the other agents" would be
pure coordination overhead for a single-user tool. This file is the orchestration
logic, written down so it's followed consistently instead of improvised each time.
Same principle applied to v3: application-strategy classification (weakness #4 from
the v3 review) is folded into `career-matching-agent`'s existing output rather than
given its own agent, since it's a derived label from data matching-agent already
computes.

## Trigger

Nicole says something like "run a job search pass," "find me some [role] openings,"
or "help me get application materials ready for [posting]."

## Flow

**Stage 0 — Profile freshness check.**
Read `/resources/career-ai/candidate-profile.md` and its `profile_status`
frontmatter field. If the file doesn't exist, or `profile_status` is `stale` or
`incomplete`, or Nicole mentions something not reflected in it (new cert, new
project, changed preferences), dispatch `career-profile-agent` first and wait for it
to finish before continuing. If `profile_status` is `current`, skip straight to
Stage 1.

**Stage 1 — Discovery (parallel with Stage 0 when both are needed).**
Dispatch `career-research-agent`. If Stage 0 is also running this pass, these two can
run at the same time — neither depends on the other's output. Research Agent writes
its batch to `/drafts/career-ai/postings-<date>.md`, following the strict
`posting:` schema (id/title/company/url/market_signal).

**Stage 2 — Matching, with strategy classification.**
Once both the profile and the postings batch are ready, dispatch
`career-matching-agent`. It reads the profile, the batch, `role-signals.md`, and
`applications-log.md`, scores every posting, and now also classifies each into an
`application_strategy` (apply_now / stretch_role / networking_target / avoid) derived
from that same score and reasoning. It writes the ranked shortlist to
`/drafts/career-ai/ranked-matches-<date>.md` following the strict `posting:` +
`application_strategy` schema.

**Stage 3 — Human checkpoint.**
Present the ranked shortlist to Nicole, score and application_strategy label both
visible per posting. She picks which posting(s), if any, to pursue. Nothing proceeds
automatically past this point — this is the main decision gate in the whole
pipeline.

**Stage 4 — Materials drafting: strategy → writer → scorer loop.**
For each posting she picks:
1. Dispatch `resume-strategy-agent` first (read-only analysis). It maps the
   posting's requirements, picks the best-fit archetype from
   `/resources/career-ai/resume-profiles/` (or explicitly falls back to the raw
   profile if none fit), and writes the `resume_draft` contract
   (target_keywords, selected_skills, identified_gaps) to
   `/drafts/career-ai/<company>-<role>/resume-strategy.md`.
2. Dispatch `resume-writer-agent` with that contract. It drafts the ATS-safe resume
   and cover letter from the contract only — never re-deriving its own requirement
   map — at `resume_version` v1.
3. Dispatch `resume-ats-scorer-agent` (read-only) to score the draft: it returns the
   `ats_alignment_heuristic` total (requirement_coverage 60 / evidence_and_placement
   25 / natural_language_quality 15), its readability band
   (excellent/good/acceptable/poor), a separate format-compliance pass/fail list, and
   — when either falls short — a concrete fix list.
4. If any format-compliance item fails, or the alignment score is below
   `target_score` (75), dispatch `resume-writer-agent` again with the fix list to
   revise (incrementing `resume_version`), then re-score. If the scorer instead flags
   a `strategy_level_miss` (wrong archetype, a requirement never mapped), route back
   to `resume-strategy-agent` to re-run, not another writer revision.
5. Cap writer-revision cycles at `maximum_revision_cycles` (2). After that, stop the
   loop and present the best draft achieved, its latest score/band, its
   format-compliance results, and any remaining honest gaps — never force a third
   cycle chasing the gate.

Throughout this loop: the score is always presented as a heuristic estimate of
alignment quality, never as a prediction of acceptance or interview odds — no
universal ATS scoring standard exists across employers or products. All three agents
verify Markdown source only; none can confirm how a final exported .docx/.pdf will
actually render.

**Stage 5 — Manual submission (outside this system).**
Nicole reviews the drafts, edits as needed, and submits the actual application
herself on the job site or portal. No agent in this system submits, emails, or
auto-fills anything.

**Stage 6 — Outcome logging (whenever it happens, not part of the pipeline run).**
When Nicole reports an outcome ("got an interview," "rejected," "no response after
3 weeks," "got an offer"), append a row to `/outputs/career-ai/applications-log.md`
directly — this is a simple structured write, not a dedicated agent call. The row now
also captures `application_strategy`, `ats_score`, `resume_version`,
`market_signal`, `job_board`, `response_days`, and `interview_rounds`, so future
analysis can check whether the score or the strategy label actually predicted
anything. This log is what `career-matching-agent` reads on future runs to spot
response patterns.

## Failure handling at the playbook level

- If Stage 1 (Research) comes back with zero postings across every channel, stop
  before Stage 2 and tell Nicole — don't feed an empty batch into Matching.
- If Stage 0 (Profile) can't get a usable source, stop and ask her for one rather
  than proceeding with an incomplete profile silently.
- Partial channel failure inside Research (e.g. LinkedIn unreachable but Indeed
  worked) is not a stop condition — the pipeline continues with what it has and
  reports what was skipped.
- If Nicole doesn't pick anything at Stage 3, the pipeline simply ends there — that's
  a normal outcome, not a failure.
- If resume-strategy-agent finds no good archetype fit, that's not a failure either —
  it explicitly falls back to the raw profile and says so; the pipeline continues.

## Growth path

Once `applications-log.md` has real volume (dozens of logged outcomes with a mix of
responses/rejections/interviews) and the richer per-row fields (ats_score,
application_strategy, resume_version) are populated, it becomes worth exploring an
actual trained matching model fed by that log — and, per the v3 review, worth
checking whether the current rubric assumptions actually hold (e.g. whether a higher
ats_score really correlates with more interviews). Until then,
`career-matching-agent`'s explicit, visible reasoning over the log is the system's
learning mechanism — intentionally simple, because there's no data yet to justify
more. Vector databases, knowledge graphs, fine-tuning, and interview-prediction
models remain explicitly out of scope until that data exists.
