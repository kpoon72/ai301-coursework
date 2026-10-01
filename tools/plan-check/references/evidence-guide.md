# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: eval, the candidate plan's "Diagnosis" (or first
  paragraph) and the plan comment's cause claim, read against the
  "Repro evidence" block's numbered steps, its "Control" runs, and its
  "Actual" line; maintainer causes are in "Thread highlights" (role in
  parentheses). Live, the draft plan.md and comment.md against the
  student's posted repro comment and the issue thread.
- What good looks like: the stated cause explains the failing steps and
  is consistent with every control. A control that shows the blamed
  part working (the same items parse without the flag; top-level
  collect gives `[null]`; FES fires on instance methods; pyarrow's typed
  parse already lost the zeros) means the diagnosis is wrong.
- Warning signs: "red herring", "side effect", "simply absent",
  blaming a stage after the one a repro step pins.

## Scope

- Where it lives: the plan's "Scope"/"In scope"/"Not in scope" lines,
  its "Proposed changes" list, and its "Files and areas" list.
- What good looks like: one change at the failing site plus regression
  tests, with larger ideas named as deferrals. Auditing the same
  pattern in the same function (sibling `cursor_max - cursor` sites) is
  still one change.
- Warning signs: "rebuild", "unify", "migrate", "upgrade", "restructure
  into modules", a new option/setting/prop, a CI matrix, "while I'm in
  there", "fixes the class rather than the instance".

## Executability

- Where it lives: the plan's "Files" and "Approach"/"Steps" sections.
- What good looks like: each step names a file and function or code
  area and the edit to make there (add `--` before the path in the
  decompress command builder; clamp with `saturating_sub` at the fill).
  Naming the file and component, with the exact function to be pinned
  by a stated method (debug logs, tracing a named call), is fine.
- Warning signs: steps that are only "investigate", "look into",
  "profile", "try different X", "fix once the cause is clear"; "not
  sure which layer"; "somewhere"; "maybe also".

## Test plan

- Where it lives: the plan's "Test plan" section, compared with the
  repro evidence's commands and outputs.
- What good looks like: the repro command re-run with the exact
  expected output, value, exit code, or count (`[[0,1,2,3,5,7,8,9]]`,
  exit 0, 10 of 10 syncs, the classList is empty), plus named
  regression tests; controls expected unchanged.
- Warning signs: "should feel fast", "look much better", "work
  correctly", "nothing else should feel broken".

## Honesty

- Where it lives: the plan's "Risk"/"Unknowns"/"open question" lines,
  and every certainty claim in the diagnosis and comment.
- What good looks like: unverified details are labelled ("I have not
  yet verified which layer clamps the viewport"; "not yet measured the
  cost"). "Risk: none identified" with a reason is fine.
- Warning signs: "the root cause is X, not Y" with nothing in the
  package showing X; "confirmed" next to a claim the repro does not
  cover. An honest mid-build deviation is recorded in plan.md under
  "Deviations" (live mode).

## Comms

- Where it lives: eval, the "Candidate plan comment" against "Thread
  highlights" (author roles, stated causes, suggested directions,
  linked PRs/patches) and the repo-facts "contribution policy" line.
  Live, the draft comment.md against the issue thread and the repo's
  CONTRIBUTING / AI policy files.
- What good looks like: the comment follows or explicitly responds to
  any maintainer direction or patch ("along the lines already agreed
  here", "option 2 from the discussion", "I have read #3543"), names
  its own scope, and promises report-back on a condition, not a date.
  Where the policy requires disclosure of AI use in comments (or all AI
  use), the comment has a disclosure line naming the tool and extent.
- Warning signs: a docs-only or different plan on a thread where the
  owner already located the culprit and posted a patch, with no mention
  of it; "PR up this week"; no disclosure under an all-AI-use policy.
- Not a problem: disclosure asked only for pull requests; no stated AI
  policy; non-maintainer comments the plan does not mention.
