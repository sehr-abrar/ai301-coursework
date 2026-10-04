# Rubric: is this pull request ready to submit?

Five required checks, one per way a pull request wastes a reviewer's
time, plus two preferred checks that never change a verdict. The five map
onto the failure families the eval set is built around: plan fidelity
(silent-drift), test evidence (not-tested), diff quality (unreviewable),
and the repo's stated standards (standards-wall); a package that trips
none of them is a clear-accept.

Every pass condition judges the artifact against the plan, the issue, and
the repo's stated asks, never the write-up's shape. A terse complete PR
is ready; a long confident one can be hiding drift. A PR that does less
than everything but says so honestly is ready; one that silently does
more or less than its plan is not.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan | The unified diff's changed files and hunks, read against the plan-context block's scope pair (in-scope and not-in-scope) and any deviation notes the plan records. | Passes when every change in the diff is either inside the plan's stated scope or covered by a recorded deviation note, AND the plan's intended change is actually present (or its absence is a noted deferral). Fails when the diff does more than the plan without a note (an unrequested rewrite, a bundled rename or refactor, reworded unrelated lines) or less than the plan with no deferral note. A disclosed deviation re-ties a mismatch and passes; a silent one fails. | required |
| description-matches-diff | The PR description's claims about what it changes, read against what the diff actually contains. | Passes when the description claims neither more nor less than the diff delivers: every change the description names is in the diff, and every substantive change in the diff is accounted for in the description (a disclosed limitation counts as accounting for it). Fails when the description claims fidelity the diff contradicts, advertises work the diff does not do, or stays silent about substantive changes the diff makes. | required |
| evidence-decisive | The test-evidence section, read against the plan's test plan (every trigger and failure mode it names) and the reproduction's own steps, plus the outcome of the repo's own checks or suite. | Passes when the evidence re-runs the trigger(s) the plan's test plan names, exercising the path the fix changed, and shows an observable before and after for each (the symptom present before, the expected-after present after), AND the repo's own tests or checks are shown run with their outcome visible. Fails when the evidence proves nothing observable ("tested locally", "verified working", "tests pass" with no before/after), exercises a path the fix did not change, omits a failure mode the plan's test plan explicitly named, or never runs the repo's checks. | required |
| diff-reviewable | The unified diff and the commit list. | Passes when the diff contains the change and nothing that buries it: no debug or print leftovers, no commented-out or dead code, no stray formatting churn across untouched lines, no drive-by edits unrelated to the fix. Fails when such debris is present, even alongside a correct fix. This reads the diff's hygiene; whether the change matches the plan is `diff-within-plan`. | required |
| standards-and-disclosure | The repo-facts block's PR-template asks and contribution policy (including any AI-use disclosure requirement), read against what the PR's description and commits actually contain. | Passes when every requirement the repo states for a submission is satisfied in the PR: each required template section or checklist item is filled with real content, a required changelog/whatsnew entry is present if the diff warrants one, and **where the policy requires disclosing AI assistance, the description discloses it; because all work in this course is AI-assisted, an absent disclosure under such a policy fails.** Where the repo states no such requirement, this check passes on that axis; silence is not a requirement, and matching a template's headings cosmetically is not the same as filling them. | required |
| commits-scoped | The commit list, read against the diff and the plan. | Passes when the commits describe the change they carry and none is obvious debris (a "wip", "fixup", or "revert me" left in history). Ranks accepted PRs only. | preferred |
| reviewer-ready-description | The description, read against what a reviewer needs to start. | Passes when the description names the issue it closes and states the approach, so a reviewer can orient without reading the whole diff first. Ranks accepted PRs only. | preferred |

## Verdict rule

The verdict is `accept` (ready to submit) when **every required check
passes**. Any single required check graded `fail` produces `reject`.

`unclear` on a required check counts as `fail`: a pull request whose
fidelity, evidence, hygiene, or standards compliance cannot be verified
from the package is not ready to submit. The one place silence is a pass,
not an `unclear`, is written into `standards-and-disclosure`: where the
repo states no requirement on a given axis, that axis passes.

The two preferred checks never change the verdict. Report their grades,
and use them to separate a PR that is merely submittable from one that is
easy to review.
