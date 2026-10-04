# Evidence guide: where evidence lives in a PR package

The map for `rubric.md`. For each family of evidence a check names, this
file says where to find it in a PR package and what a reader should be
able to conclude once they have it. The four families are the harness's
failure categories: plan fidelity is silent-drift, test evidence is
not-tested, diff quality is unreviewable, standards and comms is
standards-wall. A package that trips none is a clear-accept.

One rule runs through every section: the plan is the promise and the diff
is the delivery, so the load-bearing reads are side-by-side. Never grade
the diff on its own; grade it against what the plan said it would be.

## Plan fidelity (harness category: silent-drift)

**Where it lives.** The plan's scope pair (its in-scope change list and
its not-in-scope line) and any deviation notes sit in the plan-context
block (live: `plan.md`, deviation notes included). What actually changed
is the unified diff's file headers and hunks (live: `git diff main...HEAD`).
The description's fidelity claims are in the PR description. Read all
three together.

**What good looks like.** Every changed file and hunk falls inside the
plan's stated boundary, or is covered by a recorded deviation note; and
the change the plan promised is actually in the diff, or its absence is a
noted deferral. Silent drift shows in two directions. More than the plan:
an unrequested rewrite, a bundled rename or refactor, reworded unrelated
lines riding alongside the real fix. Less than the plan: a plan promising
two things (say a warning and a docs update) whose diff delivers one, with
no note. A deviation note re-ties either mismatch and makes it honest; the
absence of a note is what fails. The description belongs here too: a
description claiming more or less than the diff delivers is silent drift,
not a comms problem.

## Test evidence (harness category: not-tested)

**Where it lives.** The candidate PR's test-evidence section (live: your
captured test output), read against the plan's test plan and the
reproduction's own steps in the plan-context block. The repo's own-check
outcome is in the same section (live: the suite command the repo's README
or CI config names, run on your branch).

**What good looks like.** Two things must both be present. First, the
reproduction's own trigger re-run with an observable before and after: the
issue's symptom shown present before the change and the plan's
expected-after shown present after, on the same input the issue names, not
a paraphrase. Second, the repo's own tests or checks shown run with their
outcome visible (a pass count, a green suite). The not-tested tells: "I
tested locally", "verified working", or "tests pass" with no before/after
artifact; evidence that exercises a path the fix did not change (a GET
when the fix was on the POST path, the unchanged branch); the repo's
suite never run. An honest failing-check note with its reason is evidence;
a silent omission is not.

## Diff quality (harness category: unreviewable)

**Where it lives.** The unified diff itself and the commit list.

**What good looks like.** The change is visible and nothing buries it: the
fix stands alone, with no unrelated passengers. The debris tells are
concrete and each one fails the check on its own: leftover debug prints or
logging, commented-out or dead code, a `DEBUG`/`TODO`/`XXX` marker left
in, formatting churn rewriting untouched lines, a drive-by edit to a file
the fix did not need. This family reads hygiene only: a change that is
clean but does the wrong work is caught by plan fidelity, not here, and a
change that is correct and in-scope but drags debris along fails here even
though the fix itself is right.

## Standards and comms (harness category: standards-wall)

**Where it lives.** The repo-facts block's stated asks: its PR-template
sections or required checklist, its contributing instructions, and its
contribution policy including any AI-use disclosure requirement (live:
`.github/PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING.md`, `AI_POLICY.md`,
and explicit maintainer direction in the issue thread). Read them against
the PR's description and commits.

**What good looks like.** Every requirement the repo actually states is
honored. A required template checklist has its items addressed with real
content, not left as empty boilerplate; a required changelog or whatsnew
entry is present when the diff fixes a bug; explicit maintainer direction
in the thread is engaged. And the disclosure: where the policy requires
disclosing AI assistance, the description discloses it, naming that the
work is AI-assisted, because every submission in this course is. A policy
that requires disclosure and a description that carries none is the
standards-wall signature, no matter how strong the fix (pandas' required
checklist and ghostty's AI policy are the two live cases). Silence is
permission: where a repo states no template and no disclosure rule, there
is nothing to honor and the PR passes on that axis. Mirroring a template's
headings while leaving them empty is not honoring it; filling what they
ask for is. (Whether the description's claims match the diff is plan
fidelity, not this family.)
