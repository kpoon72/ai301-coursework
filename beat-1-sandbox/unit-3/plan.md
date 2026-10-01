# Plan: #72 `verify_password` fails closed on malformed stored hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72
My repro comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5881947784

## Repro evidence this plan relies on

From my repro comment (macOS 26.6.1 arm64, Python 3.13.13, passlib 1.7.4,
bcrypt 4.3.0, commit `2f4e82f`):

```
$ .venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" --runxfail
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified

.venv/lib/python3.13/site-packages/passlib/context.py:1132: UnknownHashError
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
```

Control from the same report: a real bcrypt hash behaves correctly,
`verify_password("password", hash_password("password"))` is `True` and
`verify_password("wrong", hash_password("password"))` is `False`.

## Diagnosis

`verify_password` in `core/security.py` returns
`bool(pwd_context.verify(plain_password, hashed_password))` with nothing
around it. When passlib can't identify the stored hash, `CryptContext.verify`
raises `UnknownHashError` from `identify_record` (the traceback above) and
the exception escapes to the caller. The control shows the comparison
itself is fine for a valid hash, so the defect is only the unhandled
exception on an unrecognizable hash, not the hashing or the comparison.

While planning I also checked two other malformed stored hashes directly
against `pwd_context.verify` on the same setup:

```
'$2b$12$PPkcSxF8xtCNf'          -> builtins.ValueError : salt too small (bcrypt requires exactly 22 chars)
'$2b$12$!!!!!!!!!!!!!!!!!!!!!!!' -> builtins.ValueError : invalid characters in bcrypt checksum
''                              -> passlib.exc.UnknownHashError : hash could not be identified
```

A truncated or corrupted bcrypt string is identified as bcrypt and then
raises a plain `ValueError`. `UnknownHashError` is itself a subclass of
`ValueError` (checked: its MRO is `UnknownHashError -> ValueError`), so
catching `ValueError` covers both shapes of "malformed stored hash".

## Scope

In scope: make `verify_password` return `False` when passlib raises
`ValueError` (which includes `UnknownHashError`) for the stored hash, and
remove the strict `xfail` marker on `test_verify_with_wrong_hash_format`
(manifest H-05), as the issue and `docs/CONTRIBUTING.md` ask.

Not in scope: `hash_password`, the `CryptContext` configuration
(schemes, `deprecated="auto"`), the passlib/bcrypt versions (including the
"(trapped) error reading bcrypt version" warning passlib prints with bcrypt
4.3.0), the login route in `api/routes/auth.py`, and any logging.

## Files

- `core/security.py`: `verify_password` only.
- `tests/unit/test_security.py`: remove the `xfail` marker from
  `test_verify_with_wrong_hash_format`; add one test for a truncated bcrypt
  hash.

## Approach

1. In `core/security.py`, wrap the `pwd_context.verify(...)` call in
   `verify_password` in `try/except ValueError`, returning `False` in the
   except branch. Add a one-line comment that `UnknownHashError` subclasses
   `ValueError`. No new imports are needed.
2. In `tests/unit/test_security.py`, delete the `@pytest.mark.xfail(...)`
   decorator on `test_verify_with_wrong_hash_format`, leaving the test body
   unchanged.
3. Add `test_verify_with_truncated_bcrypt_hash`, which passes
   `hash_password("password")[:20]` and asserts the result `is False`.

## Test plan

1. Re-run my repro test without `--runxfail` (the marker is gone):
   `.venv/bin/pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format"`
   Expected: `1 passed` (before the fix it was `1 xfailed`, and
   `1 failed` with `UnknownHashError` under `--runxfail`).
2. Re-run my repro's direct call:
   `verify_password("password", "not_a_valid_bcrypt_hash")`
   Expected: prints `False`, no traceback.
3. Re-run the control: expected output unchanged, `True False`.
4. New test `test_verify_with_truncated_bcrypt_hash`: `1 passed`.
5. Whole file, `.venv/bin/pytest tests/unit/test_security.py`: all pass,
   0 xfailed.

## Risks and unknowns

- Catching `ValueError` also catches passlib's `PasswordSizeError` (a
  password over passlib's size limit). With the fix that returns `False`
  instead of raising. I think that's the right fail-closed behavior for a
  verify function, but it's a behavior change beyond the issue's exact
  input, so I'll call it out in the PR.
- I haven't run the API. From reading `api/routes/auth.py:80`, login calls
  `verify_password` directly, so I expect a malformed stored hash currently
  makes login error out instead of returning a normal failed login. I'm not
  claiming that as verified, and the route isn't in scope.
- PR #78 from a classmate targets the same issue. Under the Path Review
  house rules it doesn't block this plan; I'm building my own change from
  my own repro.

## Deviations

Nothing changed; the plan held. The build on
`fix/72-verify-password-fail-closed` is the three approach steps as written:
the `try/except ValueError` in `verify_password`, the `xfail` marker removed
from `test_verify_with_wrong_hash_format`, and the new
`test_verify_with_truncated_bcrypt_hash`. Every test-plan result came out as
expected (H-05 `1 passed`, direct call prints `False`, control still
`True False`, `tests/unit/test_security.py` 26 passed with 0 xfailed). The
only edit after the plan check was to the comment, not the plan: I pasted the
`ValueError` output for the truncated hash into comment.md so the comment
shows the evidence it mentions.
