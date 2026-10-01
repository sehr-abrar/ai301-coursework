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

sehr-abrar

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5836520937

Following up on my reproduction above with a plan.

Diagnosis: reading the `phone_us` pattern in `safety/pii_scrubber.py`, it separates its digit groups with `[-.]?`, which matches a dash or a dot but has no alternative for a space. That appears to be why `(555) 123-4567` slips past the `)`-to-`123` seam while `555-123-4567` is caught, and why `test_us_phone_formats` also fails on the space-separated `+1 555 123 4567`. I will confirm it against a fix rather than assert it.

Plan: widen the separator classes in that one pattern so a space is accepted alongside the dash and dot, covering all four formats the tests list (dashed, parenthesized, dotted, and space-separated), then remove the four `xfail` markers on the phone tests the issue names once they pass. Scope is that single pattern; I am not touching the other PII types or `detect()`'s return shape.

On the fifth `#53`-tagged test raised in the thread (`test_mixed_pii_and_text`): I ran it, and its failure is over-redaction from a different pattern, not a missed phone number. The `street_address` regex matches "5 years developing Python appl" (the "pl" in "applications" hits its `Pl`/Place suffix), so `"Python"` gets redacted. That is a separate defect outside this issue's scope, so I plan to leave that marker in place and flag it separately rather than bundle a street_address fix into the phone change.

One thing I will confirm while building: whether to also consume the literal `(`/`)` so the redaction reads cleanly, or leave the minimal digit match. I will re-run the full `test_pii_scrubber.py` to make sure the widen does not regress the formats that already redact, and report back with the before/after once the branch is up. Open to a different direction if you would rather see this handled elsewhere.

---

## Your branch

**Branch**

fix/53-phone-space-separator

**Evidence**

My Unit 2 reproduction steps, re-run against the built change. Environment:
macOS 15.6, Python 3.12.3, fresh virtualenv with `structlog` and `pytest`.

**Before** (branch at `main`, before the change):

```
$ python -c "from safety.pii_scrubber import PIIScrubber; s=PIIScrubber(); print('scrub :', s.scrub('Call me at (555) 123-4567 or 555-123-4567')); print('detect:', s.detect('Call me at (555) 123-4567'))"
scrub : Call me at (555) 123-4567 or [REDACTED]
detect: []

$ python -m pytest tests/unit/test_pii_scrubber.py -k phone -v
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction XFAIL [ 16%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats XFAIL [ 33%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_international_phone_redaction PASSED [ 50%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii XFAIL [ 66%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text XFAIL [ 83%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_end_of_text PASSED [100%]
================= 2 passed, 19 deselected, 4 xfailed in 0.08s ==================
```

The parenthesized number survives `scrub()` and `detect()` returns `[]`;
the four issue-named tests xfail.

**After** (branch `fix/53-phone-space-separator`, with the change and the
four markers removed):

```
$ python -c "from safety.pii_scrubber import PIIScrubber; s=PIIScrubber(); print('scrub :', s.scrub('Call me at (555) 123-4567 or 555-123-4567')); print('detect:', s.detect('Call me at (555) 123-4567'))"
scrub : Call me at [REDACTED] or [REDACTED]
detect: [{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]

$ python -m pytest tests/unit/test_pii_scrubber.py -k phone -v
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction PASSED [ 16%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats PASSED [ 33%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_international_phone_redaction PASSED [ 50%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii PASSED [ 66%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text PASSED [ 83%]
tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_end_of_text PASSED [100%]
======================= 6 passed, 19 deselected in 0.05s =======================
```

Both numbers are now redacted and `detect()` finds `(555) 123-4567` as
`phone_us`. All six phone tests pass, including the four whose markers were
removed. Full-file run: `24 passed, 1 xfailed` — the one remaining xfail is
`test_mixed_pii_and_text`, which still fails on the unrelated
`street_address` over-match and keeps its marker, as planned.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Before spending any credit I hand-graded the four calibration packages against my draft
rubric and procedure. That produced no agreement score, but it exercised each family and
confirmed the trap: `calib-03` adopts the thread's confident key-binding diagnosis, but
its own timing matrix shows the 26-second cost with no pager in the loop at all (step 3,
`--paging=never`), so my `diagnosis-grounded` check rejected it on the control run rather
than following the thread. All four calibration verdicts matched the gold labels, so I
made no rubric change before running.

Scored runs, in order:

1. **20/20 scored items, bar: PASS.** Categories: clear-accept 7/7, scope-creep 4/4,
   thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. Every category matched,
   including the 2-package `thread-convention` category the eval README flags as the live
   case this week.

This is the run recorded in `eval-run.txt`, and 20/20 is its agreement line. There was
one scored run because the first one agreed with every gold label; there was nothing to
revise.

**Package analysis**

**`pkg-20`** (ghostty). **My rubric: `reject`. Gold label: `reject`. Agreed.**

`pkg-20` is the most instructive package in the set precisely because it is an excellent
plan that still fails. Eight of my nine checks pass it: the diagnosis is grounded (the
no-hyperlink control isolates growth-during-print as the trigger, matching the plan's
`prev`-staleness claim), the scope is bounded (it excludes the unconditional-recompute
approach the thread already rejected and any wider page-memory refactor), the approach is
committed (a monotonic generation counter with snapshot-and-compare in `Terminal.print`),
the test plan is observable (`zig build test` with both fuzz cases passing and the control
unchanged), the unknowns are honest (per-print cost "not yet measured", other stale
pointers "an open question"), the comment engages the maintainer's direction directly
(it answers mitchellh's hot-path cost concern), and it even engages the named prior-art
corpus (#11249).

It fails on exactly one required check, `repo-conventions-met`. ghostty's stated policy
(`CONTRIBUTING.md` + `AI_POLICY.md`) requires disclosing all AI usage, naming the tool and
the extent of assistance, and the candidate plan comment contains no disclosure at all.
Because every plan in this course is AI-assisted, an absent disclosure under a policy that
demands one is a real violation, not a stylistic gap, so the verdict is `reject` no matter
how strong the plan is. This is the category floor working as designed: a rubric with no
conventions check would have accepted `pkg-20` on the strength of its other eight checks
and missed the whole `thread-convention` category.

**Check rationale**

The check that decided `pkg-20`, quoted as it currently reads in
`tools/plan-check/rubric.md`:

> | repo-conventions-met | The repo-facts block's contribution policy and stated asks, read
> against what the plan comment actually contains. | Passes when every requirement the
> repo's stated policy places on a contributor's comments is satisfied. **Where the policy
> requires disclosing AI assistance, a plan comment that discloses none fails; all work in
> this course is AI-assisted, so an absent disclosure under such a policy is a real
> violation.** Where the policy states no such requirement, this check passes; silence is
> not a requirement, and mirroring template headings is never required. | required |

It reads this way because the `thread-convention` category has only two packages and no
volume to hide behind: one package (`pkg-04`) fails on ignoring the owner's in-thread fix
direction, and the other (`pkg-20`) fails on this disclosure requirement, so the check has
to catch the disclosure case cleanly or the category floor goes unmet. The wording is
deliberately asymmetric on silence: a policy that *requires* disclosure turns an absent
disclosure into a failure, but a policy that says nothing is permission, not an omission.
Without that second half the check would wrongly reject every plan comment in a repo with
no AI policy, which is most of them, including my own Path Review issue.

**Trade-offs**

What `repo-conventions-met` gives up is that it reads only the *stated* policy in the
repo-facts block; it cannot judge an unwritten norm. If a project expected AI disclosure
as an unstated community convention, this check would pass a comment that omitted it,
because there is no policy line to fail against. I accepted that deliberately: the
alternative is to have the skill infer disclosure obligations a repo never wrote down,
which would reject honest contributors on the grader's guesswork rather than on evidence,
and every other check in this rubric is built to grade evidence rather than vibe. The
`pkg-20` versus `pkg-04` split confirms the check is not merely a disclosure detector: on
the same run it also rejected `pkg-04` on the thread axis (`comment-engages-thread`) while
`repo-conventions-met` passed it, so the two comms checks divide the `thread-convention`
category between them rather than one carrying both. Nothing else in the run moved, because
there was only one run: the first full run agreed on all twenty packages.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
