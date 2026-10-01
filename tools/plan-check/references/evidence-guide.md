# Evidence guide: where evidence lives in a plan package

The map for `rubric.md`. For each family of evidence a check names, this
file says where to find it in a plan package and what a reader should be
able to conclude once they have it.

One rule runs through every section: the reproduction evidence is the
ground truth the plan is measured against. The plan does not get to
assert its own facts; it earns them by following from what the repro
block, the issue, and the thread already establish.

## Diagnosis and grounding

**Where it lives.** The plan's stated cause is in its diagnosis
sentence(s), usually the opening of the candidate plan. The evidence it
must answer to is the repro-evidence block: the failing run, and
especially any control runs (the "control run" / "second control" lines
that show what succeeds). In live mode the cause is in the draft plan
and the grounding is the student's posted repro comment on the issue.

**What good looks like.** Lay the stated cause beside the control runs
and check they agree. A grounded diagnosis blames the one thing that
differs between the failing run and the passing control: if the control
that succeeds removes a background fill and the failing run has one, a
cause blaming the background-fill subtraction fits. The failure signature
to catch is the cause a control run refutes: the plan blames component X,
but a control that exercises X still succeeds (or a control that avoids X
still fails). Blaming something no run in the evidence exercised is the
same failure in softer form.

## Scope

**Where it lives.** The plan's change list ("Change, one fix:", "Files:")
and its not-in-scope line ("Not in scope:", "defers", "I will not"). Read
both against what the reproduced bug actually requires.

**What good looks like.** One bounded change that removes the reproduced
failure, and nothing the bug does not require. A plan that names adjacent
work and explicitly defers it is bounding itself well, and passes. The
creep signature is additive: the isolated fix arrives bundled with a
dependency migration, a refactor of the surrounding component, a rewrite
of a state machine, or a sweep across sites the reproduction never
touched. The test: strike everything the reproduced failure does not
require, and see whether a coherent single change remains. If it does and
the plan already drew that line, pass; if the extra work is load-bearing
to the plan, fail.

## Executability

**Where it lives.** The plan's files/areas line and its approach
statement: what will be changed and how. In live mode, the same lines of
the draft plan.

**What good looks like.** A stranger could open the named file and begin,
without writing back to ask what the author meant. Two things must both
be present: a named target (a file, a function, a specific site) and a
committed approach (what will be done there). The unbuildable signature
is the deferred decision wearing a plan's clothes: "profile and then
optimize", "investigate the input stack", "recover somewhere",
"upstream or vendored, whichever is easier". Naming alternatives is fine
only when the plan also states the condition that chooses between them
("normalize in the reader if the value is set there, otherwise at the
call site"); listing options and leaving the pick open is not a plan, it
is a research task.

## Test plan

**Where it lives.** The plan's test-plan line, read against the
repro-evidence block's own steps and artifacts (the command that
triggered the failure, the exit code, the output).

**What good looks like.** The test re-runs the reproduction's own trigger
and names an observable result that tells fixed from not-fixed: the same
command now exits 0, the string is now redacted, the value is now N. The
strongest test plans also re-run the control runs unchanged to show
nothing else moved. The empty signature is a test that would pass
regardless: "make sure it works", "run the tests", "confirm the fix",
with no re-run of the reproduction and no observable after-state named.

## Honesty

**Where it lives.** The plan's risks and unknowns lines, and after a
build, its deviations note. Read each against what the reproduction and
the code actually establish.

**What good looks like.** A genuine unknown is labelled as one and, ideally,
carries how it will be resolved ("whether other underflow sites exist; I
will grep and note them in the PR"). An honest mid-build deviation, "the
plan said X, the code needed Y, here is why", is a full pass: plans meet
reality, and recording the collision is the work. The failure is
false confidence: an unknown asserted as a settled fact, a cause stated
as proven when the evidence only suggests it, an outcome promised the
reproduction cannot support.

## Comms

**Where it lives.** The plan comment, read against two things: the thread
highlights (live: the issue thread) for maintainer direction, and the
repo-facts block's contribution policy and stated asks for conventions,
including any AI-use disclosure requirement. In live mode the policy is
in the repo's `CONTRIBUTING.md`, `AI_POLICY.md`, `AGENTS.md`, and PR/issue
templates.

**What good looks like.** Two separate reads.

For the thread: if a maintainer has said something load-bearing, isolated
a culprit, proposed an approach, posted a patch, asked for prior art, the
comment engages it, whether by adopting it or by giving a reason not to.
The signature failure is a comment that steers around explicit owner
direction: the owner has isolated the code fault and posted a test
binary, and the comment proposes a docs-only workaround as if that
direction did not exist. When the thread carries no maintainer direction,
there is nothing to engage and the comment passes on this axis; silence
is not a failure.

For the conventions: read what the repo actually requires of a
contributor's words. A stated AI-use disclosure is a requirement, not a
convention, and because every plan in this course is AI-assisted, a
comment that discloses nothing under such a policy fails. Silence is
permission: most repos require no disclosure, and their absence of a
disclosure line is fine. Template headings are convention, not
requirement; answering what a template asks about, in the comment's own
arrangement, meets the project where it lives.
