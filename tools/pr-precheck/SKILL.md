---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this pull request
ready to submit? A PR package is a candidate pull request (its title,
description, commit list, diff, and test evidence) read against the
plan it claims to implement (with that plan's deviation notes and the
repro evidence it built on) and against the issue and repo it lands in
(the thread, the PR template, the contributing guide and any AI
policy). Grade exactly one package per run. Do not grade the issue,
the plan, or the code style on their own merits, and do not suggest a
better fix: ask only whether this PR, as it stands, is ready to open.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** (the student's own PR, before it is opened). Read these
inputs from the student's working copy, the top folder of their fork's
clone, on their branch:

- `plan.md`: the plan the PR implements, including its `## Deviations`
  section. A house-chain student reads the house plan and house repro
  pack instead; the same checks grade them.
- The diff: run `git diff main...HEAD` (three dots) and `git log
  --oneline main..HEAD` in the working copy. That is every committed
  change the branch makes relative to `main`. Uncommitted or untracked
  work is not part of the PR; do not grade it. `plan.md`,
  `comment.md`, `pr_draft.md`, and `test_evidence.md` are never part
  of the diff, and if one shows up in it, that is debris.
- `pr_draft.md`: the first line is the PR title and the rest is the
  description.
- `test_evidence.md`: the captured repro before/after and the repo's
  check output.
- The issue named in the request: read its body and thread with `gh
  issue view <number> --repo <scoped repo> --comments` (or the web
  page if `gh` is unavailable).
- The repo's stated asks: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` in the working copy (and any AI policy they
  link).

If `plan.md`, `pr_draft.md`, or `test_evidence.md` is missing, or the
diff against `main` is empty, stop and say which input is missing
instead of grading. In live mode the course always requires an AI-use
disclosure in the PR description (in Notes for Reviewers), so treat
the AI-disclosure ask as stated even where the repo's own docs are
silent.

**Eval mode** (a package bundle, `pkg-NN` or `calib-NN`). The bundle
text is the whole world: its issue, thread highlights, repo facts,
plan context, and candidate PR are the only evidence. Fetch nothing,
run no commands against any repo, and read no other files. Do not
grade the live issue the `source:` line names. Eval mode always grades
a complete package: every rubric check, then the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before any other
input. If its `Repo:` line still holds the `<ORG>/<PATH-REVIEW-REPO>`
placeholder, stop without grading and tell the student to fill the
`Repo:` line in `scope.md` with their section's Path Review repo.
Never guess a scope. If the issue URL in the request belongs to any
other repo, refuse to grade and say the PR is outside scope. Apply the
house rules scope.md lists while reading evidence: the PR comes from a
`fix/<issue-number>-<slug>`-shaped branch on the student's fork, it
delivers one plan for one issue, every template section gets real
content, and a classmate's PR on the same issue is not a blocker. In
eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading the rubric checks, read `voice-guide.md`
and hold the draft PR title and description against each of its rules
and its "Things I never post" list. In the summary, list every rule
the draft breaks, quoting the rule and the offending line. A broken
voice rule never changes the verdict by itself, because no rubric
check reads the voice guide; it is advice to the student. In eval
mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order, and use them as follows:

1. `rubric.md` defines the checks (name, evidence, pass condition,
   weight) and the verdict rule. It is the only source of checks:
   never add, drop, or rename one.
2. `references/evidence-guide.md` says where each evidence family
   lives in a bundle and in a live working copy, and what good looks
   like there.
3. `procedure.md` is the operating procedure. Execute it as written:
   its read order, its gathering steps, its check execution, and its
   verdict assembly.

Where the procedure is silent on something you need to do, say so in
the summary as a procedure gap and take the narrowest step the rubric
text supports; never invent a new check or a new pass condition. If
`rubric.md` has no checks in its table or no verdict rule, or
`procedure.md` has no steps, refuse to grade: say which file is
empty and stop, without emitting a verdict.

## Verdict and output

The verdict is binary: `accept` means the PR is ready to submit, and
`reject` means hold it and fix what failed. There is no third verdict
and no score. Reservations go in check evidence lines, not the
verdict.

Before the JSON you may print a short readable summary: one line per
check with its grade and evidence, the deciding check, and in live
mode the voice-guide notes and the concrete fixes. Then end the reply
with this fenced JSON block, with one entry per rubric check in rubric
order. It must be valid, it must be last, and nothing may follow it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode `item` is the issue URL (the PR is not opened yet); in
eval mode it is the bundle id.

## Grading discipline

- Evidence first: grade no check without naming the fact, quote, file,
  or hunk that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff against the plan, the
  evidence against the test plan, and the description against the
  diff. A terse PR whose diff, evidence, and asks line up is ready; a
  confident, well-formatted PR whose diff does something else is not.
  Never pass a check because the description says it would pass.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it passes. Note the tension in the
  summary; the fix belongs in the rubric.
- The procedure decides how, not you: follow `procedure.md` and
  report its gaps instead of papering over them.
- Unclear: treat it as the rubric's verdict rule directs. Where the
  rule is silent, an unclear grade counts as fail: a claim you cannot
  verify from the package is a claim the PR does not yet support.
- An honestly disclosed shortfall is not a failure: a PR that leaves
  something out and says so, in the plan's deviation notes or the
  description, can be ready.
