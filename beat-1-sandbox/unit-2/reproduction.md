# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

kpoon72

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5881939893

I'd like to take #72 as my first Path Review contribution. I can see several classmates have claimed it too; per the course house rules I'll still do my own setup and post my own report.

What I'll look at: `verify_password()` in `core/security.py` lets passlib's `UnknownHashError` escape when the stored hash isn't a format passlib recognizes, when it should fail closed and return `False`. The covering test is `test_verify_with_wrong_hash_format` in `tests/unit/test_security.py`, currently a strict `xfail` (manifest H-05), which calls `verify_password("password", "not_a_valid_bcrypt_hash")`.

I haven't reproduced it yet. Next I'll fork the repo, set it up from its docs, and run that test with `--runxfail` on current `main`. I'll post a repro report here with my environment, the exact steps, and the output I get.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5881947784

Reproduction report for #72. **Result: reproduced** on current `main`: `verify_password()` raises `passlib.exc.UnknownHashError` for a malformed stored hash instead of returning `False`.

**Environment**

- macOS 26.6.1 (build 25G76), Apple Silicon (arm64)
- Python 3.13.13 (Homebrew)
- passlib 1.7.4, bcrypt 4.3.0, pytest 9.1.1
- Code: my fork `kpoon72/pathreview-ai301-fa26-s3` at commit `2f4e82f` (same as upstream `main`, no local changes)

**Steps** (from a fresh clone of the fork)

I followed the Python part of `make setup` from `docs/SETUP.md`. I skipped Docker, the migrations, and the frontend, since `core/security.py` and this unit test don't touch them.

```bash
python3.13 -m venv .venv
.venv/bin/python -m pip install --upgrade pip setuptools wheel
.venv/bin/pip install -e ".[dev]"
```

1. Run the covering test as it's committed (strict `xfail`, manifest H-05):

```
$ .venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format"
======================== 1 xfailed, 1 warning in 1.77s =========================
```

2. Run it again with the xfail marker ignored, so the real behavior shows:

```
$ .venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" --runxfail
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified

.venv/lib/python3.13/site-packages/passlib/context.py:1132: UnknownHashError
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
========================= 1 failed, 1 warning in 0.25s =========================
```

3. Calling the function directly, with a control on a real bcrypt hash first:

```
$ .venv/bin/python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")), verify_password("wrong", hash_password("password"))); verify_password("password", "not_a_valid_bcrypt_hash")'
True False
Traceback (most recent call last):
  ...
passlib.exc.UnknownHashError: hash could not be identified
```

**Expected:** `verify_password("password", "not_a_valid_bcrypt_hash")` returns `False`, the same way a wrong password does, which is what the function's docstring promises ("True if password matches, False otherwise").

**Actual:** the call raises `UnknownHashError` from passlib's `identify_record` (shown above). A valid hash works as expected (`True` for the right password, `False` for a wrong one), so the failure is specific to a hash passlib can't identify.

Next I'll look at how `verify_password` calls `pwd_context.verify` and what it should catch so it fails closed.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first rubric: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, every
   category matched (clear-accept 8/8, disclosure 1/1, no-evidence 4/4,
   unfollowable-comms 3/3, wrong-target 4/4).
2. `--only pkg-16,pkg-19,pkg-13,pkg-04,pkg-20` after narrowing the claim check:
   5/5 agreeing (partial run, not scored).
3. Confirming full run, saved with `--save-run`: `agreement: 20/20 scored items  (bar:
   18/20: PASS)`, every category matched. This is the committed `eval-run.txt`.

**Package analysis**

`pkg-16` (pandas-dev/pandas#66656). My rubric's decision: **reject**. Gold label:
**reject** (category: wrong-target). The report looks like a clean reproduction: the
issue's exact three lines, and a traceback, `ValueError: Length of new names must be 1,
got 3`, that is the crash the issue describes. What's wrong is the environment line:
`pandas 1.5.3 (pip)`. The issue was confirmed "on the latest version and on the main
branch", and the repo facts show the latest release is v3.0.5. A crash on a release two
majors old doesn't show the bug exists in pandas today, and the report never names that
difference. My Environment recorded check fails exactly this case in clause (c) (the
version tested is OLDER than the version the issue reports the bug on), and in the final
run that was the only required check it failed, with the grader's evidence reading "Report
tests pandas 1.5.3 (pip) while the issue confirms the bug on latest/main (repo latest
release v3.0.5); no acknowledgment of the version gap."

In my first full run pkg-16 also rejected, but reading the per-check results showed it
failed a second check for the wrong reason: the grader read "I'd like to pick this up as a
first pandas contribution" as "declares self-assignment." The verdict agreed with gold, but
that reading would reject almost every honest claim, including my own. That's what drove
the revision below.

**Check rationale**

Quoted from `rubric.md`, the Claim comment specific and scoped row's pass condition:

> Passes when the claim names something specific to this issue (its component, behavior,
> command, or a pointer from the thread) and states a concrete next step that is
> investigation or verification. Fails if any of: (a) it is a +1, "any updates?", or
> complaint with no next step; (b) it asks a maintainer to assign the issue or reserve it,
> or says "assigning myself"; (c) it promises a fix by a date or "guaranteed"; (d) it is
> generic boilerplate that would read the same on any issue. Does NOT fail: stating the
> intent to work on it ("I'd like to take this", "I'd like to pick this up", "I'm working
> through this"), which is what a claim is; mentioning a planned fix direction as a plan
> ("make it warn or stash with --include-untracked"); saying "report back"; being short.

Clause (b) first read "it asks to be assigned or to have the issue reserved, or declares
self-assignment." "Declares self-assignment" was too loose: on pkg-16 the grader counted
"I'd like to pick this up" as self-assignment. So I narrowed (b) to the two things that
actually overstep, asking a maintainer to assign or reserve (pkg-19: "Kindly assign it to
me... keep this issue reserved for me") and announcing it ("Assigning myself to this",
pkg-13), and added the "Does NOT fail: stating the intent to work on it" clause so the
ordinary claim sentence is named as fine. The "Does NOT fail" list is the lesson I carried
from Unit 1: a check that only lists failures lets the grader fill the gap with its own
judgment, and it over-fires.

**Trade-offs**

Narrowing clause (b) loosens the claim check, so before the confirming run I re-ran
`--only pkg-16,pkg-19,pkg-13,pkg-04,pkg-20`. pkg-19 and pkg-13 are the packages that
failed on assignment wording; pkg-04 fails the same check on "+1 ... any updates?"; and
pkg-20 is the single disclosure package, whose claim passes this check and must still
reject on the AI-policy check. All five still rejected. pkg-19 and pkg-13 still failed the
claim check on their assignment lines, and pkg-16 now fails only Environment recorded.
Nothing changed in the full run either (20/20 both times).

What it gives up: the check now trusts wording. A claim that says "I'd like to take this"
and then quietly acts as the owner, for example by telling others to stay off it without
the words "assign" or "reserve", passes clause (b). I accept that miss: in Path Review a
classmate's claim doesn't block anyone anyway, so the house rules limit how much damage
an over-reaching claim can do.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
