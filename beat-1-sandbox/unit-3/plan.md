# Plan: fix #53 — PIIScrubber does not redact parenthesized US phone numbers

## Diagnosis

Grounded in my posted reproduction (issue #53 comment). The `phone_us`
pattern in `safety/pii_scrubber.py` is:

```
\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b
```

The parentheses are already optional (`\(?`, `\)?`), so they are not
what breaks it. The separator between the digit groups is `[-.]?`, which
matches a dash or a dot but has no alternative for a space. So the
parenthesized-with-space form `(555) 123-4567` fails to match at the
`) `-to-`123` seam, and `scrub()` leaves it in the clear while `detect()`
reports nothing. The dashed form `555-123-4567` matches and is redacted,
which is why my reproduction showed one redacted and one not on the same
input.

The four failing tests confirm the target: `test_us_phone_formats`
asserts that `+1 555 123 4567` (space-separated) is also redacted, so the
same missing-space-separator defect covers both the parenthesized and the
space-separated formats.

## Scope

**In scope:** the `phone_us` pattern in `safety/pii_scrubber.py`, and
removing the four `@pytest.mark.xfail(strict=True, reason="issue #53...")`
markers in `tests/unit/test_pii_scrubber.py` once the pattern change makes
those tests pass (the markers are `strict=True`, so a passing test with
the marker still present fails CI). The four are the tests the issue body
itself lists: `test_us_phone_number_redaction`, `test_us_phone_formats`,
`test_detect_phone_pii`, `test_phone_at_start_of_text`.

**Not in scope:** the other PII patterns (email, ssn, phone_intl,
street_address); the unused `pii_type` in `scrub()` noted in the source;
any change to `detect()`'s return shape. This is the one pattern that the
reproduction implicates and nothing else.

**The fifth `#53`-tagged test, and why it stays.** A fifth test,
`test_mixed_pii_and_text`, carries the same `reason="issue #53: ..."`
string on its xfail marker, and a classmate flagged it in the thread. I
ran it: it fails on *over-redaction*, not a missed phone number. The
`street_address` pattern greedily matches "5 years developing Python
appl" (the "pl" in "applications" matches its `Pl`/Place suffix), so
`"Python"` disappears and the test's `assert "Python" in scrubbed` fails.
That is a separate defect in a different pattern, the issue body does not
list this test among #53's related tests, and my phone-separator change
does not touch it. So I leave its marker in place (removing it would turn
a still-failing test red under `strict=True`) and will note it for the
maintainers rather than fold an unrelated street_address fix into a phone
issue.

## Files / areas

- `safety/pii_scrubber.py` — the `phone_us` entry in `PII_PATTERNS`.
- `tests/unit/test_pii_scrubber.py` — remove the four xfail markers.

## Approach

Widen the separator character classes in `phone_us` so a space is an
accepted separator alongside the existing dash and dot, in both the
country-code separator (`\+?1[-.]?`) and the two inter-group separators
(`[-.]?`). The four formats the tests require then all match:
`555-123-4567`, `(555) 123-4567`, `555.123.4567`, and `+1 555 123 4567`.

One detail I will verify during the build rather than assert now: whether
the redaction should also consume the literal `(` and `)` so the output
is clean (`[REDACTED]` rather than `([REDACTED]`), or whether matching the
digits is sufficient for the tests. The tests only require `[REDACTED]`
present and the digits gone, so the minimal change may leave a stray
paren; I will decide between the minimal widen and a slightly fuller
pattern based on what reads cleanly without breaking the dashed and dotted
cases.

## Test plan

1. Re-run my Unit 2 reproduction command against the built change:
   ```
   python3 -c "from safety.pii_scrubber import PIIScrubber; s=PIIScrubber(); print(s.scrub('Call me at (555) 123-4567 or 555-123-4567')); print(s.detect('Call me at (555) 123-4567'))"
   ```
   Expected after: both numbers redacted in `scrub()` output, and
   `detect()` returns a non-empty list for `(555) 123-4567`. Before, the
   parenthesized number survived and `detect()` returned `[]`.
2. Run the four previously-xfailed tests and confirm they pass with their
   markers removed:
   ```
   python3 -m pytest tests/unit/test_pii_scrubber.py -k phone -v
   ```
   Expected after: `test_us_phone_number_redaction`, `test_us_phone_formats`,
   `test_detect_phone_pii`, `test_phone_at_start_of_text` all PASS;
   `test_international_phone_redaction` and `test_phone_at_end_of_text`
   (the dashed/international cases that already passed) still PASS, showing
   the widen did not regress the formats that already worked.

## Risks and unknowns

- Widening the separator to include whitespace could in principle match
  looser digit runs that are not phone numbers (three digits, space, three
  digits, separator, four digits). I will re-run the full
  `test_pii_scrubber.py` file, not just the phone tests, to confirm no
  other assertion regresses, and note any borderline in the PR.
- The `\b` anchoring around the optional `(` means a minimal digit-only
  widen may leave the leading `(` outside the redacted span. This is the
  open question in the approach above; it is cosmetic to the redaction and
  does not affect whether the tests pass, but I want the posted behavior to
  look right.
- Widening `phone_us` to accept spaces could in principle interact with the
  `test_mixed_pii_and_text` text. I confirmed the current failure there is
  `street_address`, not phone, but I will re-check that test after the change
  to be sure the widen introduces no new phone over-match on that input.

## Deviations

The plan held; one open question it flagged got resolved during the build,
in the direction the plan anticipated.

- **Resolved the paren-consumption question toward the clean option.** The
  plan left open whether to consume the literal `(`/`)` or accept a minimal
  digit-only match that could leave a stray `(`. I tested both against the
  full test file: the minimal widen produced `([REDACTED]` for
  `(555) 123-4567` and `+[REDACTED]` for `+1 555 123 4567`, while consuming
  the wrapper produced a clean `[REDACTED]` in both cases with no new
  over-match. I took the clean option. This required one change beyond
  widening the separator classes: replacing the leading `\b` with a
  `(?<!\w)` lookbehind, so the match can start at the `(` or `+` and include
  it in the redacted span while still refusing to match digits glued to a
  word. Final pattern:
  `(?<!\w)(?:\+?1[-.\s]?)?\(?([0-9]{3})\)?[-.\s]?([0-9]{3})[-.\s]?([0-9]{4})\b`.
- **Scope held exactly.** Four xfail markers removed (the four the issue body
  names); the fifth (`test_mixed_pii_and_text`) left in place, since it still
  fails on the `street_address` over-match as diagnosed. Full-file run after
  the change: 24 passed, 1 xfailed (that fifth test), no regressions.
- **Posted intent unchanged.** The comment already flagged this decision as
  one I would make during the build and report back on, so nothing in the
  posted plan needs correcting; the before/after below is the report-back.
