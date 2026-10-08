# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- Where it lives, eval: the "Plan context" block (its "Plan:" steps,
  "Not in scope" line, "Files:" list, and any deviation or deferral
  note) against the candidate PR's "Diff" (each `+++ b/<file>` header
  and its hunks) and "Commits" list. The description's fidelity claims
  sit in the candidate PR's "Description" and "Title".
- Where it lives, live: `plan.md` (approach, files, scope, and the
  `## Deviations` section) against `git diff main...HEAD` and
  `git log --oneline main..HEAD`; the claims are in `pr_draft.md`.
- What good looks like: every changed file falls inside the plan's
  files list or a deviation note; every hunk is the planned fix, its
  test, or a repo-required entry; every promised change is in the diff
  or named as deferred with a reason; and the description claims
  exactly what the diff shows (calib-01: one deleted line, one claim).
- Silent drift, more than the plan: a commit titled "refactor",
  "rationalize naming", "harmonize wording", or "add <option/flag>";
  a new config field or CLI flag the plan scoped out; a second file
  the plan never names (calib-03's rewritten `print/property.js`).
- Silent drift, less than the plan: a promised docs or test change
  missing from the diff while the description says it is done.
- Not drift: a deferral recorded in the plan and restated in the
  description (the plan held, just smaller, and says so).

## Test evidence (harness category: not-tested)

- Where it lives, eval: the candidate PR's "Test evidence" section
  (and any before/after in the "Description") against the plan
  context's "Test plan" line and its "Repro evidence".
- Where it lives, live: `test_evidence.md` (repro before on `main`,
  after on the branch, and the output of `make test-unit`,
  `make test-integration`, `make lint`, `make typecheck`) against the
  test plan in `plan.md`; the same output pasted into the Testing
  section of `pr_draft.md`.
- What good looks like: the plan's own repro command or scenario,
  on the failing path, re-run on the branch with the exact
  after-result the plan expects (an output, exit code, count, or
  `diff` showing identical), the before shown or quoted from the
  repro, every failure mode the test plan names covered, and the
  repo's named test command shown with its result (`cargo test -p
  ignore` passes (214 tests)).
- Warning signs: "tested locally, works now", "verified working on my
  machine", "all tests pass" with no command; evidence that runs only
  a control case or a different request/path than the bug's; a test
  plan naming two repros with only one re-run.
- Not a problem: a check that fails or runs no tests, pasted as-is
  with the reason (Path Review's `make test-integration` stops with
  "no tests ran" because it has no integration tests yet).

## Diff quality (harness category: unreviewable)

- Where it lives, eval: every `+` and `-` line of the candidate PR's
  "Diff", and the "Commits" messages.
- Where it lives, live: `git diff main...HEAD` and
  `git log --oneline main..HEAD`; also confirm with
  `git diff main...HEAD --stat` that only the fix's files are listed.
- What good looks like: every added line is the fix, its test, or a
  required entry; commit messages say what each commit does; the
  `--stat` lists only the files the plan names.
- Debris tells: `eprintln!("DEBUG ...")`, `print(`, `console.log`
  left in; `// let x = ...` or `# old attempt` commented-out blocks;
  a helper nobody calls, or one under `#[allow(dead_code)]`; a new
  `TODO`/`FIXME`; import reordering, re-indents, or identical lines
  removed and re-added in code the fix does not touch; commits named
  "wip", "fix", "fmt + cleanup", "misc cleanups while debugging";
  live, `plan.md`, `pr_draft.md`, `test_evidence.md` or `comment.md`
  showing up in the diff.

## Standards and comms (harness category: standards-wall)

- Where it lives, eval: the "Repo facts" block's "pull requests:"
  line (template sections, checklist, issue-link format, changelog or
  whatsnew ask, title convention) and "contribution policy" line (AI
  policy), against the candidate PR's "Title", "Description", and
  "Diff". Maintainer direction is in "Thread highlights".
- Where it lives, live: `.github/PULL_REQUEST_TEMPLATE.md` (Summary,
  Issue with `Closes #`, Changes, Testing checklist, Screenshots /
  Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md`, against
  `pr_draft.md`. The course asks for an AI-use disclosure in Notes for
  Reviewers on every PR.
- What good looks like: every template section present with real
  content; the issue-link line names the issue; checklist boxes ticked
  only where true; a required CHANGELOG or whatsnew entry present in
  the diff; and, where the policy asks, an own-words disclosure that
  names the AI tool and what it did (pkg-11 style: "AI-assisted with
  me reviewing and understanding every line").
- Warning signs: a required checklist or `closes #` line simply
  absent; a required whatsnew/changelog entry missing from the diff;
  a placeholder section left as the template comment; no disclosure
  under a policy that requires disclosing all AI use.
- Not a problem: a repo with no template and no AI policy; process
  steps a PR text cannot show (CLA, vouch flow); the CI box left
  unticked before the PR is opened. (Whether the description's claims
  match the diff is plan fidelity, above.)
