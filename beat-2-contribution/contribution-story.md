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

The collector's health check only tells you whether the collector's main loop is still waking up. If measurement tasks pile up, for example because sources keep timing out, the loop keeps going and keeps writing a fresh timestamp, so Docker reports the container as healthy even though it's falling behind. There's no setting that marks it unhealthy when too many tasks are running at once.

**Expected Behavior**

A new environment variable, `COLLECTOR_RUNNING_TASKS_HEALTHY_THRESHOLD` (default 120), sets how many running tasks are still healthy. When `Collector.running_tasks` holds more than that, `Collector.record_health` doesn't update the health check timestamp, so after 600 seconds the health check fails and the container shows as unhealthy. The default comes from the other defaults: up to 30 new tasks every 20 seconds, each timing out after at most 120 seconds, makes at most (120/20) × 30 = 180 running tasks, and 2/3 of that is 120. The rationale goes in the code and the docs.

**Current Behavior**

`record_health` writes the timestamp on every wake-up, whatever the number of tasks. In my reproduction, with 200 tasks in `running_tasks`, the health file went from `2000-01-01T00:00:00+00:00` to the current time, and `base_collectors.config` has no threshold setting.

**Affected Components**

- `components/collector/src/base_collectors/config.py`: where the collector's environment variables are read
- `components/collector/src/base_collectors/collector.py`: `Collector.record_health` and `Collector.running_tasks`
- `components/collector/tests/base_collectors/test_collector.py`: the existing health check tests
- `docs/src/software.md` (the collector's Health check section and environment variable table) and `docs/src/changelog.md`
- Not changed but involved: `components/collector/src/healthcheck.py`, the Docker health check, which fails when the timestamp is more than 600 seconds old

**Toolkit**

- Tool: Claude Code in my quality-time clone, with my `CLAUDE.local.md` memory file loaded. I asked it to trace where the health check timestamp is written and read, and checked each answer against the code myself.
- What it produced: the four files above, plus `healthcheck.py` (the issue doesn't mention it). Knowing how that script works is what makes "don't update the timestamp" enough to mark the container unhealthy.
- Difficulties: `docs/src/software.md` says the health check is in the collector's Dockerfile, but there's no `components/collector/Dockerfile`; the actual check is `src/healthcheck.py`. The memory file sped up orientation, but I still had to correct one wrong guess in it about where that check lives.
- What changed: I recorded the 600-second limit and the `healthcheck.py` location in `CLAUDE.local.md`, so later sessions start from the right facts.

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

`Collector.start` (`components/collector/src/base_collectors/collector.py`) calls `self.record_health()` every time the collector wakes up, before it starts new tasks. `record_health` is a `@staticmethod` that writes `iso_timestamp()` to `config.HEALTH_CHECK_FILE` unconditionally. Being static, it can't see `self.running_tasks`, the set where `collect_metrics` keeps every measurement task until its done-callback removes it. `components/collector/src/healthcheck.py` only checks that the timestamp is at most 600 seconds old. So the health check measures "the loop is alive", and nothing connects it to the task count. `base_collectors/config.py` has no threshold to compare against.

**Proposed Solution**

Add `RUNNING_TASKS_HEALTHY_THRESHOLD = int(os.getenv("COLLECTOR_RUNNING_TASKS_HEALTHY_THRESHOLD", "120"))` to `config.py`, with the issue's rationale in a comment. Turn `record_health` into an instance method that returns without writing when `len(self.running_tasks)` is greater than the threshold. "Exceeds" means strictly greater, so exactly 120 is still healthy. Document the variable in the collector's environment variable table and Health check section in `docs/src/software.md`, and add a changelog entry.

Not changing: `healthcheck.py` and its 600-second limit, the WARNING in `__log_tasks` (a separate `MEASUREMENT_LIMIT` threshold), how tasks are created or cancelled, and the notifier's health check. I'm not adding any new logging either, since the issue doesn't ask for it.

**Implementation Plan**

1. In `components/collector/tests/base_collectors/test_collector.py`, add a test that fills `running_tasks` with one more task than a patched `RUNNING_TASKS_HEALTHY_THRESHOLD` and asserts the health file is not opened, plus a boundary test where the count equals the threshold and the timestamp is still written. Run it to see it fail on master.
2. In `components/collector/src/base_collectors/config.py`, add `RUNNING_TASKS_HEALTHY_THRESHOLD` (default 120) with the rationale comment.
3. In `components/collector/src/base_collectors/collector.py`, drop `@staticmethod` from `record_health` and return early when `len(self.running_tasks) > config.RUNNING_TASKS_HEALTHY_THRESHOLD`. Keep the existing `OSError` handling as it is.
4. In `docs/src/software.md`, add the variable to the collector's environment variable table with its default and rationale, and add a sentence to the Health check section.
5. In `docs/src/changelog.md`, add an entry under `## [Unreleased]` → `### Added` ending "Closes [#8507](https://github.com/ICTU/quality-time/issues/8507)."
6. Run `just test` and `just check` in `components/collector` and `just check` in `docs`, then rerun my reproduction script on the branch.

Deviations (2026-10-28): (1) Step 4: the new row was wider than the table's columns, and markdownlint's MD060 rule needs aligned tables, so I realigned the whole collector environment variable table. Every row is re-padded, but the only new text is my row. (2) Step 5: the changelog links to `software.md#collector`, because `#health-check` would point at the Frontend's Health check section. (3) Step 1: the new tests only error with `AttributeError` until the config setting exists, so I added step 2's setting first to see the real failure.

**Toolkit**

- Tool: plan-check, with the wild scope from Unit 6. I ran `plan-check: grade my plan in plan.md and draft comment in plan-comment.md for issue <URL>` from my clone. The comment stays local and isn't posted.
- What it produced: accept on all 7 checks on the first run. It checked the plan against the source itself ("`record_health` is indeed a bare `@staticmethod` called unconditionally in `start()`") and noted that no maintainer had commented yet, so there was no direction for the plan to follow or depart from.
- Difficulties: it reported "`voice-guide.md` does not exist in the installed skill directory", so it skipped the voice pass. The file was there, but as the symlink from the Unit 6 install, and the skill's file search doesn't follow symlinks. An accept that silently skips part of its check is easy to miss.
- What changed: I replaced the symlinks in repro-check, plan-check, and pr-precheck with INSTALL.md's fallback, a one-line file pointing at `~/.claude/voice-guard/voice-guide.md`, and ran the voice guard's manual check on `plan-comment.md`, which passed. No plan changes were needed.

---

## Testing Strategy

Four things prove the fix, each with output I can show:

1. **My reproduction, rerun before and after.** The same script (200 pending tasks in `running_tasks`, an old timestamp in the health file, then `record_health()`) on master and on my branch:

   ```
   master: health file after:  2026-10-28T23:31:07.077992+00:00
           timestamp updated (container reported healthy): True
   branch: health file after:  2000-01-01T00:00:00+00:00
           timestamp updated (container reported healthy): False
   ```

2. **A new test that fails without the fix.** `test_skip_writing_health_check_when_too_many_tasks_are_running` patches the threshold to 2, adds 3 running tasks, and asserts the health file is never opened. With only the config setting added and `record_health` unchanged, it fails with `AssertionError: Expected 'open' to not have been called. Called 1 times.` With the fix, it passes.
3. **A boundary test.** `test_writing_health_check_when_running_tasks_equal_threshold` adds exactly 2 tasks with the threshold at 2 and asserts the timestamp is still written, because the issue says "exceeds". The two existing health check tests still pass unchanged, which shows that turning `record_health` into an instance method didn't break them.
4. **The repo's own checks, from my ship manifest, each in a fresh shell.** `just test` and `just check` in `components/collector` and `just check` in `docs` all exit 0, the full collector suite is `Ran 776 tests ... OK` (774 on master plus my 2), and markdownlint on `software.md` and `changelog.md` exits 0.

**Toolkit**

- Tool: my ship manifest's checks, run by hand in fresh login shells (`env -i HOME=$HOME /bin/zsh -lc`), the same way the ship-gate runs them, plus my repro script and the new unit tests. Everything went into `test_evidence.md` in my clone.
- What it produced: before/after output, the new test failing on the real behavior and then passing, and exit 0 on all four manifest checks.
- Difficulties: the docs check passed while hiding a real failure. `docs/package.json` runs `markdownlint src/**/*.md; lockfile-lint`, so the `;` makes `npm-lint` report OK even when markdownlint fails. On my Mac, markdownlint was actually crashing on Node v19. Once I ran it on Node 26, it failed my first version of the new table row (MD060, columns not aligned). It also reports 20 old errors in `deployment.md` and `versioning.md`, which fail on master too.
- What changed: I ran markdownlint as its own manifest row on the two files I change, using Homebrew's Node 26, and moved the whole-docs run into the excluded table with the date and the master failures as the reason. I realigned the table so it passes, and added `SSL_CERT_FILE=/etc/ssl/cert.pem` to the docs row, since vale's setup download failed on python.org's Python certificates.

---

## Implementation Notes

**2026-10-28**

Synced my clone with upstream `master` (now `c4cb9d77d`) and made the branch `fix/8507-running-tasks-health`. I wrote the two tests first. On unchanged code they only errored with `AttributeError` (the setting didn't exist), which doesn't test the bug. So I added the `config.py` setting on its own and ran them again: the over-threshold test then failed for the real reason, with `open` called once. Then I changed `record_health`. I made it an instance method and return before touching the file, not after the `try`, so the existing `OSError` handling stays exactly as it was. I used real `asyncio` tasks in the tests instead of `Mock` objects, because `running_tasks` is typed `set[asyncio.Task]` and the repo runs mypy and ty on tests.

**2026-10-28**

The docs took longer than the code. My first version of the new environment variable row broke markdownlint's aligned-table rule, because the variable name is wider than the column. I realigned the collector's table, which re-pads every row, though the only new text is my row. That's a deviation from the plan, which only said "add the variable". I also shortened the row's description so the table didn't get much wider. In the changelog I first linked `software.md#health-check`, then found that `software.md` has nine "Health check" headings and the first one is the Frontend's, so I linked `#collector` instead.

**Toolkit**

- Tool: Claude Code in my clone, with `CLAUDE.local.md` loaded, writing the changes one plan step at a time. I ran the tests after each step.
- What it produced: the diff (config setting, `record_health` change, 2 tests, docs row and sentence, changelog entry) and the before/after test runs.
- Difficulties: two edits were refused because the file hadn't been read in that session yet. One refusal came back while a "before the fix" test run was already going, so that run used the old file. I only caught it because the output was the same `AttributeError` as before. The table formatting was invisible until markdownlint actually ran.
- What changed: after every edit, I check the test output against what that edit should change before moving on, and I added the realigned table and the `#collector` anchor to the plan's deviations.

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
