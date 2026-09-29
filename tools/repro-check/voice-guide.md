# Voice guide: how I talk upstream

## Who I am in threads

I'm a student making my first contributions. My hands-on experience is
React/JS/TS and I'm using this course to get better at Python backends.
When I comment, readers should expect a small, checked claim about what I
actually ran and what I'll look at next, nothing bigger.

## Rules I write by

### Rule: Promise the investigation, not the fix

I commit to the next thing I will look at and to reporting back. I never
promise a fix, a PR, or a date before I've reproduced and understood it.

- Wrong: "I'll have a fix up for this by the weekend."
- Right: "Next I'll read `verify_password` in `core/security.py` and post what I find."

### Rule: Only say "reproduced" when the output is pasted

If I haven't run it yet, I say I plan to. If I ran it, the output goes in
the comment. Words like "confirmed" need an artifact beside them.

- Wrong: "I've confirmed this bug and I know what's causing it."
- Right: "I haven't reproduced it yet; I'll set up the repo from its docs and post the repro report here."

### Rule: Name this issue's specifics

Every comment mentions something only true of this issue: the function,
the error, the test. If the comment would fit on any issue, rewrite it.

- Wrong: "This looks like a good issue for me, I'd like to work on it!"
- Right: "I'd like to take this: `verify_password` raising `UnknownHashError` on a malformed hash instead of returning False."

### Rule: Hypotheses stay labelled as hypotheses

When I guess at a cause, I say it is a guess and what would check it.

- Wrong: "The problem is that passlib throws on unknown hashes."
- Right: "My guess is the `UnknownHashError` isn't caught; I'll check that against the xfail test."

## Things I never post

- "Assign this to me" / "please reserve this for me"
- A deadline or "guaranteed"
- "+1", "same here", "any updates?", or "same as above, can confirm"
- "Reproduced" or "root cause" without pasted output
- Flattery in place of content ("great project, love it")
