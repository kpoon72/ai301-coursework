# Procedure: how this skill grades a plan package

## Read order

1. Read the issue first: its title, body, the error or wrong behavior it
   describes, and the expected behavior. Note in one line what the bug is.
2. Read the thread highlights next. For each comment, note the author's
   role (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR, NONE) and whether it
   states a cause, suggests a direction, links a PR/patch, or rejects an
   approach. Maintainer (OWNER/MEMBER/COLLABORATOR) statements are the
   ones that bind the comment check later.
3. Read the repro evidence before the plan. List every numbered step and
   every control run, and for each control write down what it shows
   WORKING. The controls are what a diagnosis must not contradict, so
   they have to be in hand before the plan's claims are read.
4. Read the repo-facts block and note the contribution policy's AI rules
   in one line: none / disclosure in PRs only / disclosure required for
   comments or all AI use.
5. Only now read the candidate plan (diagnosis, scope, files, approach,
   test plan, risks), then the candidate plan comment.
6. In live mode, before step 1 read `scope.md` (refuse issues outside the
   scoped repo) and after step 5 read `voice-guide.md`. The repro
   evidence is the student's posted repro comment on the issue, or the
   repro evidence quoted in the drafts.

## Evidence gathering

For each check, pull exactly these facts (see
`references/evidence-guide.md` for where each lives):

1. Diagnosis fits the repro: quote the plan's one-sentence cause. Next
   to it, list each control run from Read order step 3 and mark whether
   the cause is consistent with it. Also note any maintainer-stated
   cause from step 2.
2. Change targets the cause: quote the plan's change in one line and
   the location the diagnosis/evidence points at; note whether they are
   the same code.
3. One bounded change: list every numbered change or "proposed change"
   and every file/area. Mark each item as: the fix, a test, a sibling
   site of the same pattern, or something else (rewrite, migration,
   upgrade, new option, abstraction, CI rework, unrelated fix).
4. A stranger could start: for each approach step, record the file,
   function or area, and the concrete edit. Record any step that is an
   investigation without a stated change, and any "not sure" / "maybe".
5. Test plan is observable: quote the test plan's expected result(s)
   and mark each as concrete (output/value/exit code/test name) or an
   impression.
6. Unknowns stated honestly: quote every certainty claim ("root cause
   is", "confirmed", "red herring") and whether evidence in the package
   backs it; quote the risks/unknowns if any.
7. Comment respects thread and repo: from the comment, record whether
   it mentions or follows each maintainer statement from Read order
   step 2, whether it has an AI disclosure line, and any date promise.

In live mode, gather issue and thread facts from GitHub (issue page or
API) and the repo's CONTRIBUTING / AI policy files, and the repro
evidence from the student's repro comment or the drafts.

## Check execution

1. Run the checks in rubric order: Diagnosis fits the repro, Change
   targets the cause, One bounded change, A stranger could start, Test
   plan is observable, Unknowns stated honestly, Comment respects thread
   and repo.
2. For each check, apply its pass condition to the facts gathered for
   it: first test each numbered "Fails if" clause; if one matches,
   before failing, test the "Does NOT fail" clauses; if one of those
   covers the case, the check passes.
3. Grade pass or fail with a one-line evidence quote naming the clause
   that decided it. Grade unclear only when the fact the check needs is
   absent from the package (e.g. no test plan section at all), and say
   what is missing.
4. Grade every check even after one fails; later checks never change an
   earlier grade. Re-read a package section only when the gathered facts
   for a check do not decide it.
5. Grade the plan's content, not its format: a plan written as one
   paragraph is read the same as one with headings.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check
   is pass. Any fail or unclear on a required check makes the verdict
   reject.
2. In the summary, name the first failing check in rubric order as the
   deciding check and quote its evidence line; list any other fails
   after it.
3. In live mode, list any voice-guide rule the draft comment breaks,
   quoting the rule; this never changes the verdict.
4. Emit the JSON block from SKILL.md last, with one entry per rubric
   check in rubric order.
