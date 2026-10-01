# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

kpoon72

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5922714582

Plan for #72, building on my repro above (`UnknownHashError` escaping `verify_password` on `2f4e82f`, with a valid bcrypt hash still returning `True`/`False` correctly).

**Diagnosis:** `verify_password` in `core/security.py` returns `pwd_context.verify(...)` with nothing around it, so when passlib can't identify the stored hash its `UnknownHashError` escapes. While planning I also found that a truncated bcrypt string (e.g. the first 20 chars of a real hash) raises a plain `ValueError`. Calling `pwd_context.verify("password", h)` directly on the same setup:

```
'not_a_valid_bcrypt_hash' -> passlib.exc.UnknownHashError : hash could not be identified
'$2b$12$PPkcSxF8xtCNf'    -> builtins.ValueError : salt too small (bcrypt requires exactly 22 chars)
```

`UnknownHashError` subclasses `ValueError`, so one `except` covers both.

**Change:** wrap the `pwd_context.verify` call in `try/except ValueError` and return `False`. Remove the strict `xfail` marker on `test_verify_with_wrong_hash_format` (H-05) and add one test for a truncated bcrypt hash. Nothing else changes: `hash_password`, the `CryptContext` config, and the login route stay as they are.

**Test plan:** the H-05 test goes from `1 xfailed` to `1 passed`; `verify_password("password", "not_a_valid_bcrypt_hash")` prints `False` with no traceback; the valid-hash control still prints `True False`; the full `tests/unit/test_security.py` passes with 0 xfailed.

**Open point:** catching `ValueError` also means an oversized password (`PasswordSizeError`) returns `False` instead of raising. I think fail-closed is right for a verify function, but I'll flag it in the PR. I've seen PR #78 on this issue; per the house rules I'm building mine from my own repro. I'll report back here once the tests pass on my branch.

---

## Your branch

**Branch**

fix/72-verify-password-fail-closed

**Evidence**

Environment: macOS 26.6.1 (arm64), Python 3.13.13, passlib 1.7.4, bcrypt 4.3.0, pytest
9.1.1, in my fork's clone with `.venv` from `pip install -e ".[dev]"`. The last command
prints the valid-hash control (`True False`) on its first line, then the malformed-hash
call.

**Before** (unchanged `main`, commit `2f4e82f`):

```
$ git rev-parse --short HEAD
2f4e82f
$ .venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format"
======================== 1 xfailed, 1 warning in 0.34s =========================
$ .venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" --runxfail
E           passlib.exc.UnknownHashError: hash could not be identified
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
========================= 1 failed, 1 warning in 0.26s =========================
$ .venv/bin/python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")), verify_password("wrong", hash_password("password"))); print(verify_password("password", "not_a_valid_bcrypt_hash"))'
True False
Traceback (most recent call last):
  ...
  File "/Users/kamalpoon/Desktop/AI301/pathreview-ai301-fa26-s3/.venv/lib/python3.13/site-packages/passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified
```

**After** (branch `fix/72-verify-password-fail-closed`, xfail marker removed):

```
$ git branch --show-current
fix/72-verify-password-fail-closed
$ .venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format"
========================= 1 passed, 1 warning in 0.25s =========================
$ .venv/bin/python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")), verify_password("wrong", hash_password("password"))); print(verify_password("password", "not_a_valid_bcrypt_hash"))'
True False
False
$ .venv/bin/pytest tests/unit/test_security.py::TestSecurity::test_verify_with_truncated_bcrypt_hash
========================= 1 passed, 1 warning in 0.44s =========================
$ .venv/bin/pytest tests/unit/test_security.py
======================== 26 passed, 1 warning in 6.46s =========================
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first rubric: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `clear-accept 6/7`. The miss was pkg-14 (gold accept, graded reject).
2. `--only pkg-14,pkg-01,pkg-11,pkg-04,pkg-20,pkg-15` after narrowing Unknowns stated
   honestly: 5/6 (partial). pkg-14 still rejected, now on Diagnosis fits the repro.
3. `--only pkg-14,pkg-01,pkg-07,pkg-11,pkg-16,pkg-04,pkg-20` after adding a "Does NOT fail"
   clause to Diagnosis fits the repro: 7/7 (partial).
4. Confirming full run, saved with `--save-run`: `agreement: 20/20 scored items  (bar:
   18/20: PASS)`, every category matched. This is the committed `eval-run.txt`.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174, OSC color responses leaking on reattach). Gold label:
**accept** (clear-accept). My first rubric decided **reject**; my final rubric decides
**accept**.

The plan is solid: its cause (on reattach, stdin is wired to the session before the OSC
color query responses are consumed) explains every repro step and both controls. 0.44.1
is clean, and a cache clear gives exactly one clean attach. The scope is the Unix reattach
handshake, the Windows variant is deferred with a reason, and the test plan is "5
consecutive SSH reattach cycles with no rgb strings in any pane."

In the first run it failed only Unknowns stated honestly, because the grader flagged a
supporting sentence: "fresh attach (which performs the same queries behind the loading
screen) is clean" was read as an unverified mechanism asserted as settled. When I
narrowed that check to the central cause, the same over-reading moved to Diagnosis fits
the repro: the plan's explanation of the cache control ("with an empty cache the color data
is refetched along the fresh-attach path once") was treated as a claim needing its own
proof. Both times my rubric treated the plan's *explanation* of the evidence as extra
claims to verify, when the check should ask whether any control contradicts the blamed
part. In the final run the grader's evidence reads "0.44.1 control and cache-clear control
are both explained, not contradicted."

**Check rationale**

Quoted from `rubric.md`, the Diagnosis fits the repro row's pass condition:

> Passes when the stated cause explains the repro's failing steps AND is consistent with
> its control runs. Fails if any of: (a) a control run shows the part the plan blames
> working correctly (e.g. the plan blames the collect operator but the control shows
> top-level collect returns `[null]`; blames a missing module but the control shows that
> module firing on another path; blames a tokenizer but the control shows the same items
> parsing without the flag); (b) a repro step pins where the failure happens and the plan
> puts it somewhere else (e.g. the step shows the value is already wrong before the stage
> the plan blames); (c) the plan dismisses part of the evidence as a "red herring" or "side
> effect" without explaining it; (d) the plan contradicts a maintainer's stated cause in
> the thread without addressing it. Does NOT fail: a diagnosis taken from the issue or
> thread that the repro is consistent with; naming an unconfirmed detail (the exact line,
> the exact layer) as an unknown; the plan's own account of WHY a control behaves as it
> does (e.g. why a cache clear gives one clean run) — that is the plan explaining the
> control, not contradicting it, and the check only asks whether some control shows the
> blamed part working correctly.

I built it around control runs because that's where all four wrong-cause packages give
themselves away: pkg-01, pkg-07 and pkg-11 each blame a component that a control in their
own repro shows working, and pkg-16's step 4 shows the zeros are gone before the cast the
plan blames (clause b). The examples in (a) come from those packages, so the grader
compares the plan against specific controls instead of judging whether a diagnosis
"sounds grounded". The last "Does NOT fail" clause was added after pkg-14: without it, a
plan that explained its controls was graded as if the explanation were a claim the
controls contradicted.

**Trade-offs**

Adding the "explaining a control is not contradicting it" clause loosens Diagnosis fits
the repro, and the packages it could flip are the wrong-cause ones, which all fail that
check. So before the confirming run I re-ran `--only
pkg-14,pkg-01,pkg-07,pkg-11,pkg-16,pkg-04,pkg-20`: all four wrong-cause packages plus the
two-package thread-convention category as canaries. All six still rejected and pkg-14
flipped to accept (7/7). The full run then matched every category.

What it gives up: a plan can now pass this check with an invented story for why a control
behaves as it does, as long as no control directly shows the blamed part working. A
confident but wrong explanation of a control ("the cache refetches along the fresh path")
is no longer caught here. I accept that miss because the alternative rejected good plans,
and Unknowns stated honestly still fails a plan whose *central* cause is asserted without
evidence.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
