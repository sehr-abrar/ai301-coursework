Hi, this would be my first contribution to PathReview. I'd like to take
the parenthesized US phone-number case in `PIIScrubber` (`safety/pii_scrubber.py`)
from this issue: `(555) 123-4567` passes through `scrub()` unredacted and
`detect()` reports no PII for it, while the dashed form `555-123-4567` is
redacted as expected.

My next step is to reproduce this in a clean checkout and post a report
with what I run and observe. If the cause is where it looks (the
`phone_us` pattern's separator handling), I'd like to attempt the fix
after I've confirmed the reproduction.
