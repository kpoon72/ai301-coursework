# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/108

**Branch**

`fix/72-verify-password-fail-closed`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run (20 scored packages, `--save-run eval-run.txt`): **19/20**, bar PASS.
   `categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.
   The one disagreement was pkg-05 (gold accept, graded reject).

That is the only run. Before it, I built the rubric by reading the gold-label notes
and the repo-facts, test-evidence, commit, and diff sections of all 20 packages side by
side. I didn't do any `--only` re-runs, because the full run already cleared 18/20 with
every category matched, and any edit to the graded files would change the fingerprints
in `eval-run.txt` and need a new full run. The committed `eval-run.txt` ends with
`agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

**pkg-05** (nushell/nushell#18848, clear-accept). My rubric: **reject**. Gold label:
**accept**. The harness note was `failed: Plan's repro re-run before and after`.

The plan's test plan names two scenarios: "re-run the issue's script, expect both
`atuin` rows; re-run with two same-name same-key bindings, expect one row plus a
warning". The test evidence shows the first one as a real before/after table. The second
one only appears as a sentence: "Same-key redefine prints the one-time warning." My
check's clause (c) fails a PR when "a failure mode or repro the plan's test plan names is
never re-run", and clause (a) fails "a statement instead of an observation". The grader
read that sentence as a statement, with no transcript, and failed the check.

I think the gold label is right. The second scenario isn't the bug, it's the new warning
path, and the diff adds `same_name_same_key_replaces_with_warning`, a unit test that
asserts exactly "one row plus a warning". `cargo test -p nu-protocol` passes (312 tests).
So the scenario is observed, just through a named test instead of a pasted transcript.
My rubric has no clause saying a named passing test that asserts the expected-after
counts as re-running that scenario. That missing clause is the gap.

**Check rationale**

The check, quoted from `tools/pr-precheck/rubric.md`:

> | AI use disclosed where required | The repo's stated AI policy (repo-facts contribution policy; live mode: CONTRIBUTING/AI policy plus the course's always-on disclosure ask), read against the description | Treat every PR as AI-assisted work. Passes when the repo has no stated AI policy and (live mode excepted) no disclosure is asked, OR the policy asks for disclosure and the description has a disclosure in the author's own words naming the tool and how it was used. Fails if the policy requires disclosure of AI use in PRs or "all AI usage in any form" and the description has no disclosure line, or the disclosure names neither the tool nor the extent. In live mode the course always asks for this disclosure in Notes for Reviewers, so it is always required there. Does NOT fail: a policy whose ask covers only issues or comments, when the PR text is in the author's own words. | required |

Why it reads this way: standards-wall has only two packages, and pkg-20 is the one this
check exists for. Its fix is excellent, but ghostty's policy says "All AI usage in any
form must be disclosed" and the description has no disclosure. A check that only asked
"is there a disclosure?" would be wrong in both directions, so I tied it to the policy.
The line "Treat every PR as AI-assisted work" is there because pkg-20 never says it used
AI. Without it, a grader can reason "no AI was used, so nothing to disclose" and pass it.

I rejected a simpler version that required a disclosure on every PR. Most clear accepts
(pkg-02, pkg-05, pkg-08, pkg-19) have "no stated AI policy" and no disclosure, so that
version would have rejected them all. The "Does NOT fail" clause covers pkg-11, where
ripgrep's policy is about comments being "written by humans in their own words". That
PR passes because its description is in the author's own words and says so. The
live-mode sentence is there because the course asks for the disclosure on my own PR even
though Path Review's docs don't.

**Trade-offs**

The cost is pkg-05, through clause (c) of "Plan's repro re-run before and after" ("a
failure mode or repro the plan's test plan names is never re-run"). I kept it strict
because that clause is what catches pkg-07 (it ran only "the single-file control") and
calib-04 (it re-runs repro 1 only, "silent on the --exec abort the plan named"). The
same strictness on pkg-05 turned a sentence about a scenario that a passing unit test
covers into a fail.

The fix I'd make is a "Does NOT fail" clause: "a test-plan scenario covered by a named,
passing test in the diff that asserts its expected-after". I haven't made it, for two
reasons. First, it would change `rubric.md`'s fingerprint, so the committed
`eval-run.txt` would no longer match the uploaded file without another full run.
Second, it loosens a not-tested check. pkg-14 says "Tests added; all tests pass" and adds
a test, so the loosened version would need pkg-14 and pkg-07 as canaries in an `--only`
run (plus calib-04 with `--include-calibration`). It would also need the word "named":
pkg-14's test covers header casing, not the POST Content-Type path, and only a
named-and-asserting test can show that.

The case I accept missing for now is any clear accept that proves a secondary scenario
with a unit test instead of a transcript. The flip side is that the rubric never accepts
a PR whose test plan quietly drops a failure mode.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
