# Evidence guide: where proof lives in a reproduction package

The map for `rubric.md`. For each family of proof a check names, this
file says where to look for it and what a reader should be able to
conclude once they have found it.

One rule runs through every section: locate the proof, then judge what
it establishes. Never judge the container it arrived in. A fenced code
block with nothing in it is not evidence, and a plain sentence naming
an exact input and an exact wrong output is.

## Environment

**Where it lives.** In an eval bundle: the opening lines of the
candidate repro report, wherever the report names an OS, a language
runtime, a release version, a branch, or a commit; and the repo-facts
block, whose "bug reports: template asks for..." line says which facts
this project considers load-bearing. In live mode: the student's draft
report, read against the issue body's own version statement and against
the repo's issue template on GitHub (`.github/ISSUE_TEMPLATE/`).

**What good looks like.** A reader can answer two questions: what
platform did this run on, and what state was the code in. The second
matters more. A commit SHA, a branch name, a release tag, or "the repo
at `main` as of today" all answer it; "latest version" does not,
because it does not survive the week. If the issue targets one version
and the report ran another, the report says which and why. Treat the
template's list as a guide to which facts are load-bearing here, not as
a checklist to tick: a report that identifies the code state precisely
but omits a field the template lists is still sufficient, while one
that fills every heading and never says what code ran is not.

## Steps

**Where it lives.** In an eval bundle: the body of the candidate repro
report, wherever it moves from describing the bug to saying what was
done. In live mode: the same section of the draft, read alongside the
repo's own setup documentation (`README`, `CONTRIBUTING`, any
`docs/development` page) so you can tell which steps the project
already documents and which the report must supply itself.

**What good looks like.** Trace the path from a clean checkout to the
moment the bug appears and ask, at each move, whether a stranger could
execute it without inventing anything. Commands that can be pasted are
the strongest form, but a precise action stated in prose ("open the
search bar and type a string that appears in no note") is equally
followable. The failure to look for is a step that names an outcome
rather than an action: "set up the project", "install dependencies",
"run the tests" each hide an unknown number of decisions. Also look for
state the report leans on but never creates: a fixture file, a config
value, a seeded database, an environment variable. Length is not a
signal in either direction. Three real commands beat a fifteen-step
narrative with a hole in the middle.

## Behavior shown

**Where it lives.** In an eval bundle: the artifact inside the
candidate repro report — a pasted output excerpt, a traceback, a test
failure, a described screenshot — read against the "## Issue" section's
account of the symptom. In live mode: the corresponding block in the
draft, read against the live issue body and any clarifying comments in
the thread.

**What good looks like.** Put the artifact and the issue side by side
and check that they are about the same event. The artifact should show
the issue's own triggering input going in and the issue's own wrong
result coming out. The failure mode this family exists to catch is the
adjacent hit: a report that produces *an* error, or *a* crash, and
treats it as *the* bug. A stack trace from a missing dependency, a test
failing for an unrelated assertion, an error on a different input than
the issue names — each of these is a real observation about something
else. The sharpest artifacts show the contrast in one place: the input
that misbehaves next to the neighbouring input that still behaves,
which proves the reporter found this bug rather than a broken
installation.

## Honesty

**Where it lives.** At the seam between the report's stated conclusion
(its "Result:" line, or whatever sentence announces what happened) and
the artifacts above it. In both modes, this family is graded by reading
those two things against each other, never by reading either alone.

**What good looks like.** The conclusion claims exactly as much as the
artifacts carry, and no more. Three specific gaps to look for. First,
the unbacked assertion: the report says "reproduced" but shows nothing
that demonstrates it. Second, the leap past the sample: one machine,
one run, one input, reported as "happens on every platform", "everyone
has this", "it has always been broken" — scope the evidence cannot
reach. Third, the diagnosis smuggled in as an observation: "this is
obviously the null-result handling" is a hypothesis, and it is honest
only when it is labelled as one.

A report that ran the steps carefully and did not see the bug is a
**full pass** in this family when it shows what it ran and what
happened instead. That result is genuinely useful to a maintainer: it
bounds the bug to environments unlike the reporter's. Honesty here is
about the fit between claim and evidence, not about the direction of
the finding.

## Comms

**Where it lives.** In an eval bundle: the candidate claim comment read
against the "## Issue" section, and both comments read against the
repo-facts block's "contribution policy" line, which carries any stated
AI-use policy. In live mode: the draft comments, the issue thread, and
the repo's actual `CONTRIBUTING.md`, `AI_POLICY.md`, `AGENTS.md`, and
pull-request or issue templates, where a disclosure requirement is
often one click from the front page.

**What good looks like.** For the claim: it names this issue and
nothing else. Test it by asking whether the comment could be pasted
unchanged onto a different issue — if it could, it is boilerplate, and
enthusiasm is not specificity. A claim that names the failing input,
the function, or the file has passed that test. The claim should also
promise only what its author can know: an investigation and a report.
A promised fix, a promised pull request, or a date is a commitment made
before the cause is understood.

For the policy: read what the repo actually requires of a contributor's
words, and separate requirements from conventions. A stated
AI-assistance disclosure is a requirement, and a package whose comments
never disclose fails it no matter how good the proof is. Silence is not
a requirement: most repos say nothing, and that is permission, not an
omission. Conditions such as "disclose AI use", "understand and test
what you submit", or "review AI output before submitting" are terms to
meet, not bans to walk away from. And a template's headings are
convention: mirroring them is courteous, but a report that answers what
the template is asking about, in its own arrangement, has met the
project where it lives.
