# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: eval, the repro report's "Environment:" line or table,
  read against the issue section's version/OS line and the repo-facts
  block (latest release, template asks). Live, the draft repro comment's
  environment line, read against the issue body and the repo's docs.
- What good looks like: the tool's version and the OS are named; the
  version is the one the issue reports or newer (latest release, main);
  if the issue ties the bug to a platform, build type (debug vs release,
  git main vs store) or config, the report ran there or says it did not.
  Any difference from the reporter's setup is written down, not hidden.
- Warning signs: no environment line at all; a version older than the
  issue's (pandas 1.5.3 against a bug reported on main); a Windows-only
  bug reproduced with no OS stated.

## Steps

- Where it lives: eval, the repro report's steps and the commands inside
  its code blocks. Live, the draft repro comment.
- What good looks like: a stranger with only the issue and the comments
  could re-run it, from a starting state (fresh dir, config file, input
  file) to the trigger. Inputs are pasted or pointed at exactly. The
  issue's trigger conditions (flags such as `--replace`, a driver, an
  option, the exact input) are kept, or any change is called out.
- Warning signs: "checked out our private repo", "used our internal
  config"; a command that drops a flag the issue needs; no commands, only
  a story.

## Behavior shown

- Where it lives: eval, the pasted output, error text, or log excerpt in
  the repro report, compared against the error or symptom quoted in the
  issue body. Live, the output blocks in the draft repro comment.
- What good looks like: the artifact contains the issue's own failure,
  the same panic or error text, the same wrong value, the same missing
  header, produced by the issue's input. A control run (same command
  without the trigger) that behaves correctly is strong extra proof.
- Adjacent-symptom traps: a different error message than the issue's
  (a parse error because the input was mistyped, "Invalid value" instead
  of "capacity overflow", "$b is not defined" instead of "Invalid path
  expression"); a different symptom (raw escape text scrolling instead of
  a crash); proof of setup only (`--version`, a session list) with no
  failure shown. Compare the literal text, not the report's description
  of it.

## Honesty

- Where it lives: the repro report's conclusion/"Actual" line and the
  claim comment's statements about the reproduction, each set against the
  artifacts above.
- What good looks like: every "reproduced", "confirmed", or "root cause"
  has an artifact behind it. A cannot-reproduce states the result first,
  shows the attempt's output, and names what differed from the report.
  Hypotheses are worded as hypotheses.
- Warning signs: "exactly as described" next to a different error;
  "I verified this race condition is the cause" with no code or log;
  "ran it ten times" used as a substitute for showing the right failure;
  "100% confirm" with nothing pasted.

## Comms

- Where it lives: eval, the candidate claim comment against the issue,
  and both comments against the repo-facts block's contribution policy
  and template asks. Live, the draft claim/repro comments against the
  issue thread, the repo's CONTRIBUTING and AI policy docs, and scope.md's
  house rules.
- What good looks like: the claim names this issue's specifics (the
  component, command, or a thread pointer) and one concrete investigative
  next step; it promises the investigation or report, not a fix or a
  date. Where the repo's policy asks for AI disclosure in comments or for
  any AI use, the package carries a disclosure line naming the tool and
  how it was used.
- Warning signs: "+1", "any updates?", "assign it to me", "keep this
  reserved", "fix it within 2 days guaranteed", praise with no content;
  a strict all-AI-use-must-be-disclosed policy with no disclosure line.
- Not a problem: a policy that asks for disclosure only in pull requests
  (no disclosure needed in comments); no stated AI policy.
