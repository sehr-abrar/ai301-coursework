Following up on my reproduction above with a plan.

Diagnosis: reading the `phone_us` pattern in `safety/pii_scrubber.py`, it
separates its digit groups with `[-.]?`, which matches a dash or a dot but
has no alternative for a space. That appears to be why `(555) 123-4567`
slips past the `)`-to-`123` seam while `555-123-4567` is caught, and why
`test_us_phone_formats` also fails on the space-separated `+1 555 123 4567`.
I will confirm it against a fix rather than assert it.

Plan: widen the separator classes in that one pattern so a space is
accepted alongside the dash and dot, covering all four formats the tests
list (dashed, parenthesized, dotted, and space-separated), then remove
the four `xfail` markers on the phone tests the issue names once they
pass. Scope is that single pattern; I am not touching the other PII types
or `detect()`'s return shape.

On the fifth `#53`-tagged test raised in the thread
(`test_mixed_pii_and_text`): I ran it, and its failure is over-redaction
from a different pattern, not a missed phone number. The `street_address`
regex matches "5 years developing Python appl" (the "pl" in "applications"
hits its `Pl`/Place suffix), so `"Python"` gets redacted. That is a
separate defect outside this issue's scope, so I plan to leave that
marker in place and flag it separately rather than bundle a street_address
fix into the phone change.

One thing I will confirm while building: whether to also consume the
literal `(`/`)` so the redaction reads cleanly, or leave the minimal
digit match. I will re-run the full `test_pii_scrubber.py` to make sure
the widen does not regress the formats that already redact, and report
back with the before/after once the branch is up. Open to a different
direction if you would rather see this handled elsewhere.
