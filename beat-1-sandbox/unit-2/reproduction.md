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

sehr-abrar

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5816823321

Hi, this would be my first contribution to PathReview. I'd like to take the parenthesized US phone-number case in `PIIScrubber` (`safety/pii_scrubber.py`) from this issue: `(555) 123-4567` passes through `scrub()` unredacted and `detect()` reports no PII for it, while the dashed form `555-123-4567` is redacted as expected.

My next step is to reproduce this in a clean checkout and post a report with what I run and observe. If the cause is where it looks (the `phone_us` pattern's separator handling), I'd like to attempt the fix after I've confirmed the reproduction.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5816870881

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

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Before spending any credit I hand-graded the four calibration packages against my draft
rubric. That produced no agreement score, but it caught a real defect: my
`claim-promises-only-investigation` check was written to fail any claim that "promises a
fix", which would have wrongly rejected `calib-01`, a gold accept whose claim states a
plan ("find where the stash-name prompt decides to appear and make the untracked-only
case either warn or stash with `--include-untracked`"). I rewrote the check to fail only
on a committed deliverable or a time, not on naming a direction, before running the
harness.

Scored runs, in order:

1. **18/20 scored items, bar: PASS.** Categories: clear-accept 6/8, disclosure 1/1,
   no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. Category floor met,
   including the single-package `disclosure` category.

This is the run recorded in `eval-run.txt`, and 18/20 is its agreement line. I stopped
here rather than chasing 20: the bar and the category floor were both met on the first
scored run, and the two remaining disagreements are a single honest limitation I would
rather describe than tune away against the answer key.

**Package analysis**

**`pkg-09`.** **My rubric: `reject`. Gold label: `accept`.**

`pkg-09` is an honest "could not reproduce" report: the author ran the argument-size
flush scenario, showed the output (`ONE ONE ONE TWO TWO TWO` with no reordering), and
stated plainly that the differential-limit condition may not have been achieved. The
gold label accepts it, because an evidenced cannot-reproduce is a genuinely useful result
that bounds the bug.

My rubric rejected it on one required check, `artifact-shows-behavior`. That check
demands the artifact display the issue's own symptom, and a cannot-reproduce report, by
definition, shows the symptom *not* occurring. So the check that exists to catch a
confident wrong-target reproduction (a report showing the wrong thing) cannot tell that
case apart from an honest report showing the bug did not happen. My other honesty check,
`conclusion-matches-evidence`, passed `pkg-09` correctly as an evidenced cannot-reproduce
, so the two checks disagree with each other, and the required one wins the verdict. The
same mechanism produced my only other miss, `pkg-10`, also a gold-accept cannot-reproduce.

**Check rationale**

The check that produced the miss, quoted as it currently reads in
`tools/repro-check/rubric.md`:

> | artifact-shows-behavior | The output excerpt, log, traceback, or screenshot in the
> report, read against the specific symptom the issue describes. | Passes when the
> artifact displays the issue's own symptom: the same triggering input and the same wrong
> result the issue names. Fails when the artifact shows an adjacent failure instead (a
> different input, a different error, a general stack trace that many causes would
> produce), or when the report describes the behavior in prose with no artifact showing
> it. | required |

It reads this way because its whole purpose is to catch the wrong-target reproduction,
the failure mode the `wrong-target` category (4 packages) and calibration package
`calib-03` are built around: a long, confident, well-formatted report whose artifact
actually shows a different event than the issue names. `calib-03` uses a colon instead of
an equals sign in its HCL input, so its artifact is a graceful syntax error rather than
the issue's panic. Demanding that the artifact show *the issue's own triggering input and
the issue's own wrong result* is exactly what separates that package from a real
reproduction, and my run matched all four `wrong-target` packages and `calib-03` on the
strength of it.

**Trade-offs**

What `artifact-shows-behavior` gives up is the honest cannot-reproduce, and `pkg-09` and
`pkg-10` are the two packages whose result it changes. By insisting the artifact show the
issue's symptom, the check cannot pass a report whose whole point is that the symptom did
*not* appear, even when that report is careful and useful. That is a real false reject,
and it is the direct cost of the check's shape.

I did not loosen it, for two reasons. First, the obvious loosening ("pass when the
artifact shows the symptom OR the report is an evidenced cannot-reproduce") hands the
cannot-reproduce judgment to prose, which is precisely the door `calib-03` walks through:
a confident wrong-target report also reads as "here is what I ran and what I saw." The
protection against wrong-target and the false reject on cannot-reproduce are the same
edge, and I would rather over-reject an honest cannot-reproduce than let a confident
wrong-target through. Second, this limitation does not reach my live work: my `#53`
reproduction genuinely reproduces, so `artifact-shows-behavior` passes it on its own
terms. Confirming a rewrite would have cost a full $4 run to move a score that already
clears the bar, so I documented the miss instead of buying it back.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
