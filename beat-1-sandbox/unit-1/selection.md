# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

````
## Summary

All three issues are in the scoped repo (`codepath/pathreview-ai301-fa26-s3`) and pass every required check — **all three verdict: accept**. Repo-level facts are identical across all three: not archived, last push 2026-09-16 (5 days ago, well within 180 days) → Repo active passes; `docs/CONTRIBUTING.md` exists and says nothing about AI-generated contributions (no `AI_POLICY.md`/`AGENTS.md` either) → AI-contribution policy passes (silence). Both preferred checks fail identically for all three: 0/5 sampled recently-updated issues (#67, #60, #57, #69, #68) got an owner/member/collaborator reply, and the repo has 2 stars / 0 releases.

Since the preferred checks don't differentiate within this repo, I ranked the accepted issues by your fit profile (Python practice, backend preferred over frontend/docs):

**Ranked (accepted):**
1. **#72** (`verify_password` raises `UnknownHashError`) — clean, isolated Python backend fix in `core/security.py`, tightly scoped (1–2h, one xfail test to un-mark), zero comments/claims on it at all.
2. **#62** (health check references nonexistent `settings.redis_host`) — equally good Python backend fix, confirmed as a real seeded bug in `docs/CONTRIBUTING.md` itself. One classmate ("skonda29") commented wanting to work on it today, but per the Path Review house rule that doesn't block you — claim it anyway.
3. **#47** (API docs missing curl examples) — bounded and clean, but it's documentation-only, not Python, so it fits your stated goal worst of the three.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "Repo active", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16 (5 days before capture date 2026-09-21)"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Single fix in core/security.py: make verify_password fail closed on UnknownHashError, remove xfail marker on manifest id H-05; no umbrella/tracking language, no unresolved design question"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; no PRs reference #72 in this repo (search of repo PRs returned total_count 0); two 'referenced' timeline events are commits in other students' personal fork repos (rafiatasafi/ai301-coursework, xyin20/ai301-coursework), not PRs against this repo; no comments on the issue"},
      {"name": "AI-contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use restriction; no AI_POLICY.md/AI_USAGE_POLICY.md/AGENTS.md present (404)"},
      {"name": "Maintainer responsiveness", "grade": "fail", "evidence": "0/5 sampled recently-updated issues (#67,#60,#57,#69,#68) had an owner/member/collaborator comment reply"},
      {"name": "Adoption signal", "grade": "fail", "evidence": "repo stargazers: 2; releases: 0"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "Repo active", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16 (5 days before capture date 2026-09-21)"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Single fix in api/routes/health.py: use settings.redis_url instead of nonexistent redis_host/redis_port; CONTRIBUTING.md itself confirms this is seeded bug #62, a ruff attr-defined suppression tied to one location"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; no linked/open PRs; one comment today from student 'skonda29' ('would like to work on this issue') — per Path Review house rule, other students' claim comments do not block"},
      {"name": "AI-contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use restriction; no AI_POLICY.md/AI_USAGE_POLICY.md/AGENTS.md present (404)"},
      {"name": "Maintainer responsiveness", "grade": "fail", "evidence": "0/5 sampled recently-updated issues (#67,#60,#57,#69,#68) had an owner/member/collaborator comment reply"},
      {"name": "Adoption signal", "grade": "fail", "evidence": "repo stargazers: 2; releases: 0"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47",
    "checks": [
      {"name": "Repo active", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16 (5 days before capture date 2026-09-21)"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Single file, single task: add example curl commands to docs/API.md; no umbrella language, no open design question"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; no linked PRs; no comments on the issue"},
      {"name": "AI-contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use restriction; no AI_POLICY.md/AI_USAGE_POLICY.md/AGENTS.md present (404)"},
      {"name": "Maintainer responsiveness", "grade": "fail", "evidence": "0/5 sampled recently-updated issues (#67,#60,#57,#69,#68) had an owner/member/collaborator comment reply"},
      {"name": "Adoption signal", "grade": "fail", "evidence": "repo stargazers: 2; releases: 0"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `--limit 4` smoke test (first draft rubric, 6 checks): `agreement: 2/4 scored items`
2. `--only issue-01` (debugging the issue-01 miss): `agreement: 0/1 scored items`
3. `--only issue-01,issue-04,issue-05,issue-10,issue-15,issue-20` (after rewriting the
   Bounded scope pass condition to stop treating "touches several files" as an umbrella
   signal): `agreement: 5/6 scored items`
4. `--only issue-15,issue-09,issue-01,issue-04,issue-05,issue-10,issue-20` (after adding
   the "2+ closed/unmerged linked PRs" clause to Bounded scope, re-checking issue-09 as a
   canary since it also has one closed PR): `agreement: 7/7 scored items`
5. Full run, no `--save-run` (confirming before spending on the saved run):
   `agreement: 20/20 scored items (bar: 18/20: PASS)`
6. Full run with `--save-run eval-run.txt` (the committed run):
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

`issue-15`. My rubric's decision: **reject**. Gold label: **reject** (category: scope).
My rubric originally accepted issue-15 (a Zulip issue with a "good first issue" label)
because it passed every check I had written at the time — unclaimed at capture (no open
linked PR, no assignee), active repo, no policy ban. Rereading the bundle, the
`this issue:` line shows `linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123
(closed)` — two separate contributors had already opened and abandoned PRs against this
exact issue, and the 97-comment thread shows a multi-year cycle of people claiming it via
`@zulipbot claim` and then getting auto-unassigned after 14 days of inactivity, going back
to 2021. Nothing about that history shows up in "is it currently claimed" — at capture
time it genuinely was not. It only shows up if you ask "how many people have already tried
and failed here," which is a scope/difficulty question, not a claim question. I added a
check clause for 2-or-more closed/unmerged linked PRs to Bounded scope (not Unclaimed) for
that reason, and re-ran issue-15 to confirm it now reads reject.

**Check rationale**

Quoted from `rubric.md`, the Bounded scope row's pass condition:

> The issue asks for one specific, describable change. Fails only if at least one of: (a)
> the issue or a comment explicitly calls it a tracking/meta/umbrella issue, or says its
> sub-items should be split into separate PRs/issues for different people; (b) the scope
> is open-ended across an unbounded surface ("the whole codebase", "all languages",
> "anywhere it applies") with no closed list of what is in scope; (c) the thread shows an
> active, unresolved design disagreement with no maintainer-settled spec; (d) it is a pure
> usage/support question ("how do I…"); (e) a maintainer comment says the fix needs
> core/internal changes; (f) a design/product decision needed to implement it (an asset,
> API shape, UI choice, etc.) is left unresolved or marked "TBD" with no maintainer having
> settled it; (g) the issue has 2 or more closed/unmerged linked PRs from different
> contributors in its history — repeated abandoned attempts signal the task is harder or
> less bounded than its label suggests, even with no live claim today. Touching several
> files, or listing several concrete examples of one underlying bug/gap ("X is missing,
> including A, B, C"), does NOT by itself fail this check — that is one bounded piece of
> work, not an umbrella. One closed PR alone (a single abandoned attempt) does not fail
> this check either

I wrote it this long because my first version failed on adjectives ("one bounded piece of
work") without saying what does and doesn't break boundedness, and the model filled that
gap with its own judgment, which over-fired on issue-01 and issue-04 (both gold accept: a
docs task touching several files, and a bug listing several affected rule types). Each
clause (a)-(g) is a concrete, checkable signal pulled from a real gold-labeled issue in the
set (issue-05, issue-10, issue-20, issue-15) rather than a restatement of "looks bounded."

**Trade-offs**

The clause (g) threshold — "2 or more closed/unmerged linked PRs" — is a number I chose,
not one the evidence guide states outright; it only says repeated abandoned attempts are a
signal, not how many make one. I set it at 2 specifically because issue-09 (gold accept)
has exactly one closed linked PR (`conda/conda#11627 (closed)`) alongside a stale 2022
claim comment, and gold treats that repo as still worth taking. A threshold of "1 or more"
would have flipped issue-09 to reject, which is wrong; I re-ran `--only
issue-09,issue-15` together after adding the clause specifically to confirm the threshold
separates the two correctly (issue-09 stayed accept, issue-15 correctly flipped to
reject). What this gives up: an issue on its *first* abandoned attempt reads identically to
one nobody has ever touched — my rubric can't yet tell "one dropped ball" from "the first
sign of real trouble," only "repeated pattern of trouble."

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. I'm using this course to get more Python practice — my hands-on experience is in
   React/JS/TS, and #72 is a small, isolated fix in `core/security.py` (make
   `verify_password` fail closed on `UnknownHashError`, un-mark one `xfail` test). It
   reads as a 1-2 hour task, which fits comfortably in the time I have for this unit.
2. The verdict correctly identified that #72 is unclaimed (no assignee, no linked PRs,
   zero comments), the repo is active (pushed 5 days before capture), scope is bounded to
   one function, and the repo states no AI-contribution restriction. What the rubric
   couldn't weigh: it ranked #72 and #62 as an exact tie on its own checks (both pass the
   same four required checks, fail the same two preferred ones), so the ranking came from
   my stated fit profile, not from anything about the issues themselves. I additionally
   noticed, reading the live output, that #62 already has a classmate's comment saying
   they want to work on it today — allowed by the house rule, but it's still a small
   social cost (claiming into a issue someone else already flagged) that the rubric has no
   check for and that #72 avoids entirely by having zero comments.
3. Since this is a shared classroom repo that every student in the section can see and the
   house rule explicitly permits claiming an issue others have commented on, the main
   difficulty isn't contention — it's that #72 touches password-verification code, so even
   though the diff itself is small, I'll want to be careful that my fix and its test
   actually cover the malformed-hash case rather than just silencing the failure.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
