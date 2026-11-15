# Opening my first open-source fix: making quality-time's collector report unhealthy when its tasks pile up

## Overview

[Quality-time](https://github.com/ICTU/quality-time) is ICTU's open-source tool for monitoring software quality, and its collector service gathers the measurements. Its Docker health check only proved the collector's main loop was still waking up, so when measurement tasks piled up the container still reported healthy ([issue #8507](https://github.com/ICTU/quality-time/issues/8507)). I reproduced the problem, wrote two tests, and added a `COLLECTOR_RUNNING_TASKS_HEALTHY_THRESHOLD` setting (default 120) that stops the collector from refreshing its health timestamp when more tasks than that are running. I opened [PR #14917](https://github.com/ICTU/quality-time/pull/14917) on 2026-10-28. On 2026-11-02 the project's lead maintainer requested two changes, and the PR is still open while I make them. Along the way I used tools I built and tuned this term: I wrote the issue-scout's search strategy and rubric, the voice guide that checks everything I post, the ship manifest that runs the repo's checks before a PR opens, and the playbook my PR monitor drafts from.

## What happened

### Finding an issue that nobody had taken (2026-10-14 to 10-18)

My first two scout scans of well-known Python projects accepted 0 of 10 issues. In scan 1, all five picks already had open pull requests, because my ranking only looked at assignees and labels. I changed my search strategy by adding `-- -linked:pr` to every query, and the result counts showed how crowded these issues were:

```
query 1: 17 results -> 1   (16 of the 17 newcomer issues already had a PR)
query 2: 65 results -> 29
```

Scan 2 confirmed the fix: none of its five picks failed on an open PR. It still accepted nothing, because the remaining claims sat in comment text that the search results don't include. So I saved 500 issues from the course's Issue Selection site and pre-checked them through the GitHub API for the same signals (Python repo, open, unassigned, no open PR, no recent claim comment). That left 27. I hand-picked three, and the scout accepted quality-time #8507 as its top pick on 2026-10-18.

### Claiming it without overpromising (2026-10-21)

My voice guard stopped my first claim comment, under my own rule about promising the investigation and not the fix:

```
2026-10-21 20:26 [manual] stop: rule broken: "I commit to the next thing I will look at and to reporting back. I never promise a fix, a PR, or a date before I've reproduced and understood it." | target: claim-draft.md
2026-10-21 20:28 [manual] pass | target: claim-draft.md
2026-10-21 20:48 [hook] pass | target: gh issue comment https://github.com/ICTU/quality-time/issues/8507 --body-file
```

I rewrote the opening around my first step, a unit test, and the posted claim says: "My first step is a unit test in `tests/base_collectors/test_collector.py` that puts more than 120 tasks in `running_tasks` and checks whether the health file gets updated."

### Proving the bug, then the fix (2026-10-22 to 10-28)

I wrote a script that puts 200 pending tasks in `Collector.running_tasks`, writes an old timestamp to the health file, and calls `record_health()`. On master the timestamp was refreshed anyway. On my branch it isn't:

```
master: health file after:  2026-10-28T23:31:07.077992+00:00
        timestamp updated (container reported healthy): True
branch: health file after:  2000-01-01T00:00:00+00:00
        timestamp updated (container reported healthy): False
```

I wrote the tests first. On unchanged code they only errored with `AttributeError`, because the setting didn't exist yet, and that doesn't test the bug. So I added the config setting on its own and ran them again. This time the new test failed for the real reason:

```
Test that the health check file is not written when more tasks are running than the threshold. ... FAIL
AssertionError: Expected 'open' to not have been called. Called 1 times.
```

The fix itself is small. `record_health` was a static method, so it couldn't see the running tasks:

```diff
-    @staticmethod
-    def record_health() -> None:
+    def record_health(self) -> None:
+        if len(self.running_tasks) > config.RUNNING_TASKS_HEALTHY_THRESHOLD:
+            return
```

With it, the full collector suite passed: 776 tests, the 774 on master plus my two.

### A check that hid a failure (2026-10-28)

My ship manifest ran the same `just check` in `docs` that CI runs, and it said OK. But the repo's lint script is `markdownlint src/**/*.md; lockfile-lint`, and the `;` means it reports success even when markdownlint fails. On my Mac, markdownlint was actually crashing on Node v19. I added markdownlint as its own manifest row on Node 26, and it caught a real error in my change: my new row in the environment variable table broke the aligned-table rule (MD060). I realigned the table, and the ship-gate passed my branch before the PR opened:

```
- verdict: PASS
  - Collector unit tests (with coverage): `cd components/collector && just test` -> ok (18s)
  - Collector quality checks (ruff, mypy, ty, bandit, vulture, codespell, ...): `cd components/collector && just check` -> ok (13s)
  - Markdownlint on the two docs files this PR changes ...: -> ok (0s)
```

### The review (2026-11-02 to 11-08)

Four days after I opened the PR, the lead maintainer, fniessink, reviewed it and requested two changes. The first is about my config line:

> If the operator changes one or more of the MEASUREMENT_LIMIT, MEASUREMENT_FREQUENCY, or MEASUREMENT_TIMEOUT, but does not change the RUNNING_TASKS_HEALTHY_THRESHOLD it is not recalculated. Please use the calculation in the comment to set RUNNING_TASKS_HEALTHY_THRESHOLD.

The second changes my design:

> Instead of not writing the file, it would be better to record the number of running tasks in the file as well as the verdict (healthy/unhealthy) and then in src/healthcheck read that information. That way someone debugging can at least see why the collector is not healthy.

My PR description had asked whether they wanted a warning log when the check is skipped. The second comment answers that question with a better idea.

I ran my new PR monitor on the PR, and it drafted a reply that started: "The changes-requested review didn't include any comments, so I don't have anything concrete to act on yet". That was wrong. The monitor reads the PR with `gh pr view`, which returns the review's empty top-level text but not inline comments on the diff. I didn't send it, and I added a rule to my playbook: before I trust a draft, I read the inline comments with `gh api repos/ICTU/quality-time/pulls/14917/comments`.

## Limitations and what's next

My fix doesn't do what the maintainer asked yet. The threshold is hardcoded at 120, so it goes stale if an operator changes the other settings. And when the collector is unhealthy, the health file says nothing about why. The branch also now conflicts with master in `docs/src/software.md`. Next, I'll calculate the threshold from the measurement settings, write the running task count and the verdict to the health file for `healthcheck.py` to read, rebase onto master, and rerun my manifest's checks. Then I'll reply to each review comment and re-request review.

I'd also do one thing differently. My plan comment stayed on my computer, so the maintainer first saw my design in the finished PR. Posting a short plan on the issue could have caught the second point before I built it. On the tool side, my PR monitor should fetch inline review comments itself, and the scout should run the API pre-check that found this issue as a cheap stage before it grades anything.
