# Rubric: is this plan ready to post and build from?

Seven required checks, one per way a plan wastes the build that follows
it, plus two preferred checks that never change a verdict.

Every pass condition below judges the plan itself against the issue and
its reproduction evidence: whether the diagnosis follows from what was
reproduced, whether the change is one bounded thing, whether a stranger
could start building it, whether the test would show success, whether
the unknowns are named honestly, and whether the comment meets the
thread and the repo where they are. None of them counts sections,
measures length, or requires particular headings. A terse complete plan
is ready; a long confident one can be unbuildable or aimed at the wrong
cause.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause, read against the repro-evidence block, including every control run it contains. | Passes when the stated cause explains the reproduced failure AND is consistent with each control run in the evidence (the passing control isolates the same layer the cause blames). Fails when a control run contradicts the blamed cause (the control that succeeds exercises the very thing the plan blames, or the control that fails does not), or when the plan blames something the reproduction never exercised. | required |
| scope-bounded | The plan's change list and its not-in-scope statement, read against what the reproduced bug requires. | Passes when the plan is one bounded change that addresses the reproduced failure, including a plan that explicitly defers or excludes adjacent work. Fails when it bundles work the bug does not require: a refactor, a dependency migration, a rewrite of a component, or a multi-front campaign across unrelated sites. Deferring adjacent work is bounding; adding it is creep. | required |
| plan-executable | The plan's named files or areas and its chosen approach. | Passes when a stranger could start building without asking the author what to do: the plan names the file(s) or area(s) to change AND commits to one approach. Fails when the approach is deferred to investigation ("profile and optimize", "investigate the input stack", "recover somewhere", "upstream or vendored, whichever is easier") or when no target site is named. Choosing among named alternatives on a stated condition is a chosen approach; naming the alternatives and leaving the choice open is not. | required |
| test-plan-observable | The plan's test plan, read against the repro evidence's own steps and artifacts. | Passes when the test plan re-runs the reproduction's own trigger and states an observable expected-after (an exit code, a redaction, a concrete value, a specific output) that would distinguish fixed from not-fixed. Fails when the test plan names no observable result, or would pass whether or not the bug was fixed ("make sure it works", "check the tests"). | required |
| honesty-calibrated | The plan's risks and unknowns (and, after a build, its deviations note), read against what the evidence actually establishes. | Passes when genuine unknowns are stated as unknowns and the plan claims no more certainty than the reproduction supports; an honestly recorded deviation passes in full. Fails when an unknown is dressed as a settled fact, or the plan asserts a cause or outcome the evidence has not shown. | required |
| comment-engages-thread | The plan comment, read against the thread highlights (live: the issue thread). | Passes when the comment engages any explicit maintainer direction the thread contains: an owner-isolated culprit, a proposed approach, a posted patch, requested prior art. Silence passes: when the thread carries no maintainer direction, there is nothing to engage and the comment cannot fail here. Fails when the thread contains explicit maintainer direction and the comment ignores or contradicts it without saying why (proposing docs when the owner has isolated the code fault and posted a patch is the signature). | required |
| repo-conventions-met | The repo-facts block's contribution policy and stated asks, read against what the plan comment actually contains. | Passes when every requirement the repo's stated policy places on a contributor's comments is satisfied. **Where the policy requires disclosing AI assistance, a plan comment that discloses none fails; all work in this course is AI-assisted, so an absent disclosure under such a policy is a real violation.** Where the policy states no such requirement, this check passes; silence is not a requirement, and mirroring template headings is never required. | required |
| cross-references-prior-art | Any related issue, prior PR, or sibling defect the issue or thread names, read against whether the plan acknowledges it. | Passes when the plan engages named prior art (a linked sibling bug, an earlier attempt) rather than ignoring it. Ranks accepted plans only. | preferred |
| scope-honestly-reduced | The plan's not-in-scope statement, read against the issue's full ask. | Passes when the plan narrows a broad issue to a defensible first slice and says what it is deferring, rather than silently attempting everything. Ranks accepted plans only. | preferred |

## Verdict rule

The verdict is `accept` (ready to post and build from) when **every
required check passes**. Any single required check graded `fail`
produces `reject`.

`unclear` on a required check counts as `fail`: a plan whose grounding,
scope, executability, test, honesty, or comms cannot be verified from
the package is not a plan that is ready to build from.

The one silence-passes exception is written into the checks themselves:
`comment-engages-thread` passes when the thread carries no maintainer
direction, and `repo-conventions-met` passes when the policy states no
requirement. Those are genuine passes, not `unclear`.

The two preferred checks never change the verdict. Report their grades,
and use them to separate a plan that is merely buildable from one that
is easy for a maintainer to accept.
