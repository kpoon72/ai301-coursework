# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo active | Repo-facts block: `archived:` flag and `last push to any branch` date (live mode: the repo front page's archived banner, if any, and the newest commit's date) | `archived:` is `no`, AND the last push to any branch is within 180 days of the capture date (eval mode) or of today (live mode) | required |
| Bounded scope | Issue title, body, and labels, plus the comment thread (eval: the Issue and Comments sections; live: the issue page) | The issue asks for one specific, describable change. Fails only if at least one of: (a) the issue or a comment explicitly calls it a tracking/meta/umbrella issue, or says its sub-items should be split into separate PRs/issues for different people; (b) the scope is open-ended across an unbounded surface ("the whole codebase", "all languages", "anywhere it applies") with no closed list of what is in scope; (c) the thread shows an active, unresolved design disagreement with no maintainer-settled spec; (d) it is a pure usage/support question ("how do I…"); (e) a maintainer comment says the fix needs core/internal changes; (f) a design/product decision needed to implement it (an asset, API shape, UI choice, etc.) is left unresolved or marked "TBD" with no maintainer having settled it; (g) the issue has 2 or more closed/unmerged linked PRs from different contributors in its history — repeated abandoned attempts signal the task is harder or less bounded than its label suggests, even with no live claim today. Touching several files, or listing several concrete examples of one underlying bug/gap ("X is missing, including A, B, C"), does NOT by itself fail this check — that is one bounded piece of work, not an umbrella. One closed PR alone (a single abandoned attempt) does not fail this check either | required |
| Unclaimed | Repo-facts `this issue: assignees:` and `linked PRs:` lines, plus the comment thread (eval: Comments section; live: issue sidebar + comments) | `assignees:` is `none`, AND no linked PR is listed with state `(open)`, AND no claim comment ("I'll take this" / "working on this" / "can I work on this") posted within the last 12 months appears without a maintainer explicitly reopening the issue afterward. A claim comment older than 12 months with no linked PR and no recent follow-up does not block | required |
| AI-contribution policy | Repo-facts `contribution policy` line (eval mode); `CONTRIBUTING.md`, `AI_POLICY.md`/`AI_USAGE_POLICY.md`, or PR template (live mode, per `references/evidence-guide.md`) | The policy contains no outright ban on AI-generated code or documentation. Disclosure, human-review, or testing conditions pass. Silence (no stated policy) passes | required |
| Maintainer responsiveness | Repo-facts `maintainer first-response sample` (eval mode); a live sample of 5 recently updated issues (live mode) | At least 1 of the 5 sampled issues received an owner/member/collaborator reply, at any latency | preferred |
| Adoption signal | Repo-facts `repo:` line star count and `latest release` date (eval mode); repo front page star count and Releases box (live mode) | Star count is 500 or more, OR a release was published within the last 12 months | preferred |

## Verdict rule

Accept only if all four required checks — Repo active, Bounded scope, Unclaimed,
AI-contribution policy — pass. A required check graded `unclear` counts as fail: a
first issue you cannot verify is not a first issue to take.

Preferred checks (Maintainer responsiveness, Adoption signal) never change the
verdict. Among accepted issues, rank by number of preferred checks passed
(2 preferred passes ranked above 1, above 0); break ties by more recent
last-push date.
