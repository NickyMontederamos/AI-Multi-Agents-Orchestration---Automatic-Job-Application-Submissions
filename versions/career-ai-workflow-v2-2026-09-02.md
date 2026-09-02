---
name: career-ai-workflow-v2-2026-09-02
description: Version snapshot of the career-ai agent system — compact workflow reference plus a changelog of what changed from v1 (2026-09-02 ServiceNow job-search session). Source of truth stays live at /workflows/career-ai/career-acceleration-playbook.md; this file is a point-in-time record, not maintained going forward.
version: 2
snapshot_date: 2026-09-02
supersedes: v1 (initial playbook, no version file — profile/research/matching/resume agents only)
---

# Career-AI Workflow — v2 Snapshot (2026-09-02)

## What's new in this version

Built during today's ServiceNow job-search session, triggered by two real gaps that
showed up running the pipeline live:

1. **Resumes/cover letters had to ship as `.docx`, not `.md`.** Nothing in v1
   produced anything but Markdown.
2. **Every draft had unfilled `[Last Name]` / `[Phone]` / `[LinkedIn URL]`
   placeholders**, because `candidate-profile.md` had never had a place to store
   contact info in the first place — so every resume draft either guessed or left a
   gap.

Both are now closed as permanent, reusable parts of the pipeline (not one-off fixes):

| # | Change | Why |
|---|---|---|
| 1 | **Installed `resume-ats-scorer-agent`** (moved from `agents-to-install/` into `.claude/agents/`) | It existed as a spec but was never active — Stage 4's scoring loop couldn't run without it. |
| 2 | **Added `workflows/career-ai/scripts/md_to_docx.py`** (Python, `python-docx`) | Converts finished resume/cover-letter Markdown into single-column, standard-font, standard-header `.docx` — matching the scorer's own format-compliance checklist by construction. |
| 3 | **New Stage 4.5 — Export to `.docx`** | Runs after a draft clears the scoring gate (or hits the 2-cycle cap). `.md` stays the editable source in `/drafts/career-ai/`; `.docx` — the actual submission-ready file — lands in `/outputs/career-ai/`, matching this repo's drafts-vs-outputs convention. |
| 4 | **Added a `## Contact Info` section to `candidate-profile.md`** | Root fix for the placeholder problem — a canonical place for last name, phone, location, email, LinkedIn, portfolio/GitHub to actually live. |
| 5 | **New agent: `candidate-intake-agent`** | Pure deterministic logic (presence/absence checks against a fixed 6-field checklist — never judgment or guessing). Two modes: profile-check (Stage 0.5) and draft-scan (Stage 4.5, regex for `[...]` tokens). It never talks to Nicole directly — no interactive tool — it hands a precise gap list to the main session, which does the actual asking. |
| 6 | **New Stage 0.5 — Contact & link intake** | Runs once per pipeline pass, alongside Stage 0. Resolves each required contact field exactly once, ever — an explicit decline (e.g. "don't show GitHub") is a resolved state, not a gap, so it's never re-asked. |
| 7 | **Stage 4.5 folded in a reconciliation pass** | Before export, re-scans the specific draft for placeholder tokens and cross-checks against the now-current profile — if Stage 0.5 resolved a field after a draft was written, this re-injects the real value via `career-resume-agent` without re-asking Nicole. |

Net effect: a posting she picks now goes from shortlist → scored/revised Markdown →
placeholder-free `.docx` with zero manual cleanup, and every future posting after the
first one skips the contact-info interview entirely.

**Known rough edge:** a newly installed agent (`candidate-intake-agent`) did not
register with the `Agent` tool immediately after its file was created this session —
it needed an extra beat/turn before it became callable. Worth confirming next session
whether that's a one-time cache thing or a recurring pattern.

---

## Compact Workflow Reference

**Trigger:** "run a job search pass," "find me [role] openings," "get application
materials ready for [posting]."

**Agents in play:** `career-profile-agent`, `candidate-intake-agent`,
`career-research-agent`, `career-matching-agent`, `career-resume-agent`,
`resume-ats-scorer-agent`. No dedicated orchestrator — the main session reads each
agent's `description` and dispatches directly.

```
Stage 0   Profile freshness check
          candidate-profile.md's profile_status: current → skip to Stage 1
                                    stale/missing → career-profile-agent, wait

Stage 0.5 Contact & link intake (parallel with Stage 0)
          candidate-intake-agent checks 6 fixed fields (name, phone, location,
          email, LinkedIn, portfolio/GitHub) by presence only.
          missing  → main session asks Nicole directly → candidate-intake-agent
                     writes confirmed value to candidate-profile.md
          resolved/declined → skip, never re-ask
          (one-time cost per field, across all future postings)

Stage 1   Discovery (parallel with Stage 0/0.5)
          career-research-agent → /drafts/career-ai/postings-<date>.md
          zero postings across every channel → stop, tell Nicole
          partial channel failure → continue, report what was skipped

Stage 2   Matching
          career-matching-agent reads profile + batch + role-signals.md +
          applications-log.md → /drafts/career-ai/ranked-matches-<date>.md

Stage 3   HUMAN CHECKPOINT — Nicole picks posting(s), or none (normal outcome)

Stage 4   Materials drafting + scoring loop, per posting picked
          1. career-resume-agent drafts → /drafts/career-ai/<company>-<role>/
          2. resume-ats-scorer-agent scores:
               ats_alignment_heuristic /100
                 requirement_coverage 60 + evidence_and_placement 25
                 + natural_language_quality 15
               format-compliance: separate pass/fail checklist
               gate: target_score 75, both gates independent
          3. below gate or format fail → career-resume-agent revises w/ fix list
             → re-score
          4. cap at maximum_revision_cycles (2) — never force a 3rd cycle

Stage 4.5 Placeholder reconciliation + export to .docx
          candidate-intake-agent draft-scan: find [...] tokens in this draft,
          cross-check against candidate-profile.md
            resolvable → career-resume-agent re-injects real value
            unresolved (declined/optional) → leave as explicit placeholder
          main session runs workflows/career-ai/scripts/md_to_docx.py
            .md (source of truth, stays in /drafts/) → .docx (deliverable,
            → /outputs/career-ai/<company>-<role>/)

Stage 5   MANUAL SUBMISSION — Nicole reviews & submits herself. No agent submits,
          emails, or auto-fills anything, ever.

Stage 6   Outcome logging (whenever it happens)
          Nicole reports outcome → append row to
          /outputs/career-ai/applications-log.md directly (no dedicated agent)
          → feeds career-matching-agent's future runs
```

**Hard rules that don't change:**
- `fabrication_allowed: false` everywhere — a gap gets disclosed, never papered over
  with a stretched synonym.
- ATS scores are always a heuristic estimate of alignment, never a prediction of
  acceptance or interview odds — no universal ATS standard exists across employers.
- Both the scorer and the exporter only verify Markdown-source structural intent;
  final rendered `.docx` still gets a human visual check before submission.
- Stage 3 (posting selection) and Stage 5 (submission) are the two points nothing
  proceeds past automatically — one is a decision gate, one is a hard stop.

**Growth path (unchanged from v1):** once `applications-log.md` has real outcome
volume, it's worth exploring a trained matching model. Until then,
`career-matching-agent`'s visible reasoning over the log is the system's only
learning mechanism, deliberately — there's no data yet to justify more.
