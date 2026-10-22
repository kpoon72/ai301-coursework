# Contribution: Report the collector as unhealthy if the number of running tasks is high

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Kamal Poon
**GitHub Username:** kpoon72
**Issue:** https://github.com/ICTU/quality-time/issues/8507
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout's search strategy on my Unit 1 rubric, then patched both after two real scans (2026-10-14)**

I installed the scout over my Unit 1 issue-select skill and wrote a search strategy from my fit profile: `gh search issues` queries for newcomer issues and lightly discussed bugs in 15 named Python library, CLI and backend orgs (a plain "good first issue + Python" search returned 10,729 results, mostly one-person repos), ranked with newcomer labels first and fewest comments first. My first scan accepted 0 of 5, because every pick already had an open PR (poetry#8864 had three), and my AI-policy check graded `unclear` whenever a repo had no root `CONTRIBUTING.md`, even though my rubric says silence passes. I added `-- -linked:pr` to every query (query 1 dropped from 17 results to 1), and changed the rubric to also check `.github/`, `docs/` and the repo's community profile, with no policy counting as a pass. A second scan confirmed both fixes but still accepted 0 of 5, because the remaining claims sat in comment text that ranking can't read, so I'm using the scout to grade issues I pick by hand rather than to find them.

**Unit 6: Put my voice guide behind a pre-post hook and wrote a memory file for quality-time (2026-10-21)**

I merged my three copies of the voice guide into one at `~/.claude/voice-guard/voice-guide.md`, added Upstream rules for claims, questions and pings, and installed the guard as a `PreToolUse` hook; the wiring test (`gh issue comment --help`) was stopped as expected. The first real check stopped my claim draft under my own rule "Promise the investigation, not the fix" (its opening line read as promising the whole fix, even though repro-check had accepted it), so I rewrote the opening around the first step, a unit test, and it passed. I also reworded my never-post rule "A deadline" to "a deadline for a fix or PR", since a claim now has to say when I'll report back, and the old wording would have stopped "within a week". For `CLAUDE.local.md` I read the collector code myself instead of running an orientation agent, because the issue already named the files, and while writing the file I found `healthcheck.py`, the script that actually reads the health file, which the issue doesn't mention.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My two scout scans of named Python orgs accepted 0 of 10, and the first 8 picks I graded from the Issue Selection site were rejected too (open PRs, a design task with nothing decided, C++ repos I shouldn't have picked). So I saved 500 issues from the site and pre-checked them with the GitHub API for the same signals my rubric looks at: Python repo, open, unassigned, no open linked PR, no claim comment in the last year. That left 27, and I hand-picked three that matched my profile. The scout accepted all three on 2026-10-18. On 2026-10-21, after the scope patch, I graded them again: ICTU/quality-time#8507 was still accepted and ranked first, with the difficulty note **moderate** (several files, but every location is named). It's the only one of the three that passed both preferred checks (maintainers replied in 2 of 5 sampled issues, and there was a release on 2026-10-09). MetOffice/CSET#925 was accepted with "moderate-to-ambitious", and Blueprints#228 was rejected because it had been closed that morning. Before claiming, I read the whole #8507 thread (no comments, no assignee, no linked PR) and the repo's CONTRIBUTING.md and AGENTS.md, which don't restrict AI use.

The issue fits what my profile asks for: a small Python backend change in the collector, with the exact environment variable, default and method already written down by the maintainer who opened it, and an existing unit test file (`test_collector.py`) to extend. It's a feature rather than a bug fix, but it has the same shape as my `verify_password` fix: one method behaving wrongly under a specific condition (here, too many running tasks). I hope to learn how a real Python project handles config through environment variables and how a container health check works from the inside. My honest read of the scout: as a finder it failed in the wild, because during Hacktoberfest the claims sit in comment text that its ranking can't see. As a grader it was right every time I checked it by hand, including closed issues and open PRs. The pre-check I did with the API is what actually found this issue, and it should become a cheap stage in the scout before any grading.

**Claim comment:** https://github.com/ICTU/quality-time/issues/8507#issuecomment-6330724916

**Toolkit**

- Tool: issue-scout, my search strategy plus my Unit 1 rubric. I used `issue-scout: grade <URLs>` on hand-picked issues after the scans came up empty.
- What it produced: an accept for quality-time#8507 on 2026-10-18 and again on 2026-10-21, with all four required checks passing, both preferred checks passing, and the difficulty note "moderate: touches config defaults, docs, and collector health logic across files, but every location is named".
- Difficulties: the scans couldn't find unclaimed issues, because claims live in comments. My old Bounded scope check also rejected every issue with an open design edge, which in the wild is most of them. On the regrade, the Unclaimed check failed Blueprints#228 for being closed, but my rubric never actually says to check issue state; the grader had to infer it.
- What changed: I applied the Unit 6 scope patch, replacing Bounded scope with scope-feasible (umbrella issues still fail, and open edges become a difficulty note), and logged it in `scan-notes.md`. Changes I'd still make: add "issue is open" to Unclaimed, and make the API pre-check a stage before grading.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- macOS 26.6.1 (Apple Silicon), Python 3.14.7 (the collector's `pyproject.toml` requires `>=3.14`)
- uv 0.13.4 and just 1.59.0, installed with Homebrew (`brew install uv just`), the two tools the developer manual lists for running a component outside Docker
- quality-time `master` at `477db7e04c4bbb569e4fae3e4f4b81e56e4ae471` (2026-10-21)
- Setup: `git clone https://github.com/ICTU/quality-time.git`, `cd components/collector`, then `just test --coverage=no`, which creates the venv with uv and runs the unit tests (`install-py-dependencies OK`, `py-unit-test OK`; 774 tests on clean `master`).

Challenges: I had neither uv nor just installed, so I followed the manual's prerequisites. Running a single test file with plain `python -m unittest` errored on imports until I set `PYTHONPATH=src`, which the repo's `test` recipe sets for you. No Docker or MongoDB was needed, because the reproduction mocks the database.

**Steps to Reproduce**

1. From `components/collector`, run a short script (in full in my repro report) with `PYTHONPATH=src uv run python repro_8507.py`. It points `HEALTH_CHECK_FILE` at a temp file and creates a `Collector` with a mocked database.
2. The script adds 200 pending asyncio tasks to `Collector.running_tasks` (more than the issue's proposed threshold of 120), writes an old timestamp (`2000-01-01`) to the health file, and calls `Collector.record_health()`.
3. Observed result: the health file gets a fresh timestamp anyway, so the container would still report healthy, and `base_collectors.config` has no running-tasks threshold setting.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:**

```
COLLECTOR_RUNNING_TASKS_HEALTHY_THRESHOLD in config: False
running tasks: 200
health file before: 2000-01-01T00:00:00+00:00
health file after:  2026-10-22T01:24:57.386745+00:00
timestamp updated (container reported healthy): True
```

- **My findings:** The gap the issue describes still exists on `master`. `config.py` defines the three values the issue's rationale uses (`SLEEP_DURATION` 20, `MEASUREMENT_LIMIT` 30, `MEASUREMENT_TIMEOUT` 120, so at most (120/20) × 30 = 180 tasks, and 2/3 of that is 120) but has no threshold. `Collector.record_health` is a `@staticmethod`, so as written it can't see `self.running_tasks`. My guess is the fix makes it an instance method, which the existing tests should still accept since they call it on an instance. `components/collector/src/healthcheck.py` reads the same file and fails when the timestamp is more than 600 seconds old, so not refreshing the timestamp is enough to mark the container unhealthy. The separate WARNING log in `__log_tasks` (above `MEASUREMENT_LIMIT`) is a different threshold, and I'm leaving it alone.

**Toolkit**

- Tool: repro-check with the new wild scope. I ran `repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue <URL>`. Before that I ran the claim alone through repro-check and through the voice guard's manual check.
- What it produced: accept on all 7 checks for both drafts, including "Behavior matches issue: health file after: 2026-10-22... timestamp updated (container reported healthy): True, with 200 tasks and no threshold config."
- Difficulties: repro-check accepted my first claim draft, but the voice guard stopped the same draft under "Promise the investigation, not the fix". The two tools read my voice guide differently, and the stricter one was the one that runs right before posting.
- What changed: I rewrote the claim's opening around the first step instead of the fix, and both tools passed it. I kept the reproduction as a script excerpt rather than a commit, because the fork doesn't exist yet.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
