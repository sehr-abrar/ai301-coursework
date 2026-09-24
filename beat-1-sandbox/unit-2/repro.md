Reproduction report for #53. Result: reproduced independently.

**Environment**
- macOS 15.6 (build 24G84), Apple Silicon
- Python 3.12.3
- PathReview fork at commit `2f4e82f` (current `main`)
- Fresh virtualenv with `structlog` 26.1.0 (the only runtime import in
  `safety/pii_scrubber.py`) and `pytest` (a dev dependency, needed only
  for the test run below)

**Steps**
From a clean checkout of the repo:
```
python3 -m venv .venv && ./.venv/bin/pip install structlog pytest
./.venv/bin/python -c "
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print('scrub :', s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print('detect:', s.detect('Call me at (555) 123-4567'))
"
```

**Observed**
```
scrub : Call me at (555) 123-4567 or [REDACTED]
detect: []
```
The dashed number `555-123-4567` is redacted; the parenthesized
`(555) 123-4567` is left in the clear, and `detect()` returns an empty
list for it. This is the behavior the issue describes.

I also ran the phone tests verbosely:
```
./.venv/bin/python -m pytest tests/unit/test_pii_scrubber.py -k phone -v
```
```
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction XFAIL [ 16%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats XFAIL [ 33%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_international_phone_redaction PASSED [ 50%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii XFAIL [ 66%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text XFAIL [ 83%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_end_of_text PASSED [100%]
================= 2 passed, 19 deselected, 4 xfailed in 0.10s ==================
```
The four seeded phone tests fail as marked. `test_phone_at_end_of_text`,
which asserts on the dashed `555-123-4567`, still passes, so the dashed
format keeps working while the parenthesized cases do not; the failure
is specific to the parenthesized format rather than to phone handling
generally.

**Where it points (not yet confirmed against a fix)**
The `phone_us` pattern is
`\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b`.
The parentheses are already optional (`\(?` and `\)?`), so they are not
what breaks it: the separator class `[-.]?` between the groups matches a
dash or dot but has no alternative for the space in `(555) 123-4567`.
That would explain why the parenthesized-with-space form slips through
while the dashed form is caught. I have not yet verified a change
against the pattern; that is my next step, and any change there needs to
keep the dashed and dotted forms redacting.
