# Procedure: how this tool grades a PR package

## Read order

1. Live mode only, before anything else: read `scope.md` and follow
   SKILL.md's scope seam (stop on a placeholder Repo line, refuse an
   issue outside the scoped repo). Eval mode skips this step.
2. Read the issue: title, body, and the wrong behavior it reports.
   Note in one line what the bug is. Then read the thread and note any
   maintainer (OWNER/MEMBER/COLLABORATOR) direction or question.
3. Read the repo's stated asks next, before any PR text: the PR
   template sections and checklist items, the contributing guide's PR
   asks (issue link, changelog, test command, title convention), and
   the AI policy. Write them down as a numbered ask list, marking
   each as "visible in a PR" or "process only" (CLA, vouch). Live
   mode: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`,
   plus the course's always-on AI-disclosure ask.
4. Read the plan before the PR: its diagnosis, scope and "not in
   scope" lines, files, approach steps, test plan, and deviation
   notes. Write down (a) the list of promised changes and files,
   (b) the list of scoped-out items, (c) every repro or failure mode
   the test plan says it will re-run and the expected-after for each,
   (d) every deviation or deferral note.
5. Only now read the candidate PR, in this order: the diff (file by
   file, hunk by hunk), the commit list, the test evidence, and last
   the title and description. The diff comes before the description
   so the description's claims are checked against what you already
   saw, not used to explain the diff.
6. Live mode only, after grading: read `voice-guide.md`.

The order matters because every check is a side-by-side: the plan
lists from step 4 are the yardstick for the diff and the evidence, and
the diff from step 5 is the yardstick for the description.

## Evidence gathering

Build these pairings before grading (see `references/evidence-guide.md`
for where each lives):

1. Diff vs plan (checks 1 and 2): make a table with one row per
   changed hunk: file, what the hunk does, and its label: planned fix,
   test, repo-required entry, named in a deviation note, or UNPLANNED.
   Then make the reverse list: each promised change from Read order
   4a, marked present in the diff, disclosed as deferred, or MISSING.
2. Description vs diff (check 3): list every factual claim in the
   title and description, including fidelity claims ("exactly as
   planned", "no other changes", "in full"). Mark each claim as shown
   by the diff, contradicted by the diff, or absent from the diff.
3. Evidence vs test plan (check 4): for each repro or failure mode
   from Read order 4c, quote the evidence line that re-runs it, the
   exact input or command, and the after-result shown. Mark it
   re-run on the failing path, control-only or different path, or not
   re-run. Note whether the evidence is a transcript/output or only a
   statement.
4. Repo checks (check 5): quote every suite or check command named in
   the evidence with its stated result. Compare against the commands
   the repo names (live mode: all four Path Review checks).
5. Debris (check 6): scan every added and removed line for the debris
   tells listed in the rubric and record each one with file and line
   content. Record the commit messages.
6. Asks vs PR (checks 7 and 8): for each "visible in a PR" ask from
   Read order 3, record where the PR meets it (section, line, file) or
   that it is missing. Record the AI policy's ask and quote the
   description's disclosure line, or record that there is none.
7. Title (check 9): quote the title.

Live mode: get the diff and commits with `git diff main...HEAD` and
`git log --oneline main..HEAD`; the evidence from `test_evidence.md`
and any before/after pasted in `pr_draft.md`; the title and
description from `pr_draft.md`.

## Check execution

1. Run the checks in rubric order: Diff stays inside the plan, Plan
   delivered or shortfall disclosed, Description matches the diff,
   Plan's repro re-run before and after, Repo's own checks run with
   results, Diff carries no debris, Repo's PR asks honored, AI use
   disclosed where required, Title says what changes.
2. For each check, apply its pass condition to the facts gathered for
   it: test each "Fails if" clause; if one matches, test the "Does NOT
   fail" clauses before failing; if one covers the case, the check
   passes.
3. Grade pass or fail with a one-line evidence string that quotes the
   deciding fact (a hunk, a claim, a missing command) and names the
   clause. Grade unclear only when the fact the check needs is absent
   from the package (for example, no test-evidence section at all),
   and say what is missing.
4. Grade every check even after one fails; a later check never
   changes an earlier grade. Re-read a package section only when the
   gathered facts do not decide a check.
5. Do not let one finding fail every check: an unplanned hunk is
   graded by check 1, a false claim about it by check 3, and a debug
   line by check 6. Each check grades only its own evidence.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required
   check is pass. Any required fail or unclear makes the verdict
   reject. The preferred check never changes it.
2. Pick the deciding check: the first failing (or unclear) required
   check in rubric order. Quote its evidence line in the summary, then
   list the other fails after it. For an accept, say which evidence
   made the closest call pass.
3. Live mode: after the verdict, list the voice-guide rules the draft
   title or description breaks, quoting each rule; then list concrete
   fixes for each failing check. Neither changes the verdict.
4. Emit the JSON block from SKILL.md last, with one entry per rubric
   check in rubric order (nine entries), and nothing after it.
