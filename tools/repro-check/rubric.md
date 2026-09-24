# Rubric: is this reproduction package ready to post?

Seven required checks, one per way a reproduction package wastes a
maintainer's time, plus two preferred checks that never change a
verdict.

Every pass condition below judges the outcome: what the package
actually establishes when read against its issue. None of them counts
steps, measures length, or requires a template's headings. A terse
report that shows the issue's symptom is ready; a long confident one
that shows nothing is not.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The environment record in the repro report (operating system, language or runtime version, version or commit of the software under test), read against the version the issue targets and against the facts the repo's bug-report template asks for. | Passes when a reader could tell whether their own machine matches the one the report ran on: the platform and the code state under test are both identifiable, and where either differs from what the issue targets, the report says so. Fails when no version or code state is recoverable from the report at all. Missing a field the template happens to list is not a fail if the code state is otherwise identifiable. | required |
| steps-rerunnable | The reproduction steps in the report, read as a stranger holding a clean checkout and the stated environment would execute them. | Passes when every action needed to get from that clean starting state to the triggering action is present, so the reader never has to guess a command or supply missing state. Fails when a step names an outcome instead of the action that produces it ("set up the project", "run it"), or depends on a file, fixture, or configuration the report never establishes. The number of steps and their formatting are irrelevant. | required |
| artifact-shows-behavior | The output excerpt, log, traceback, or screenshot in the report, read against the specific symptom the issue describes. | Passes when the artifact displays the issue's own symptom: the same triggering input and the same wrong result the issue names. Fails when the artifact shows an adjacent failure instead (a different input, a different error, a general stack trace that many causes would produce), or when the report describes the behavior in prose with no artifact showing it. | required |
| conclusion-matches-evidence | The report's stated result (reproduced, could not reproduce, partially reproduced) held against what its own artifacts actually demonstrate. | Passes when the stated outcome is exactly what the artifacts support, no more. **An evidenced "could not reproduce" passes in full** when the report shows what was run and what happened instead. Fails when the report asserts the bug is reproduced without an artifact showing it, or generalizes past what was run ("happens on every machine", "everyone sees this", "this has always been broken"). | required |
| claim-names-specifics | The claim comment, read against the issue body. | Passes when the claim identifies this particular issue in terms taken from it — the failing input, the function, the file, or the named symptom — so it could not be pasted unchanged onto another issue. Fails when the claim is generic interest or agreement ("+1", "claiming this one", "I'd like to work on this") with nothing issue-specific in it. | required |
| claim-promises-only-investigation | The commitments the claim comment makes: the deliverables it names and any timing attached to them. | Passes when the claim commits only to work the author can actually begin now — investigating, reproducing, reporting back — and when naming the intended direction of a fix reads as a plan rather than a guarantee, which it does especially once a reproduction has already been reported. Fails when the claim commits to a deliverable or a time it cannot yet know is achievable: a promised pull request, a fix asserted as certain, or any timing commitment including soft ones ("shortly", "by the weekend"). A claim that commits to nothing beyond looking at the issue also passes here; vagueness is `claim-names-specifics`'s job, not this check's. | required |
| repo-conventions-met | The repo-facts block's contribution policy and stated bug-report asks, read against what the claim comment and repro report actually contain. | Passes when every requirement the repo's stated policy places on a contributor's comments is satisfied in the text. **Where the policy requires disclosing AI assistance, a package whose comments disclose none fails.** Where the policy states no such requirement, this check passes; silence is not a requirement. Failing to mirror the template's headings or section order is never a fail here — only stated requirements count. | required |
| reduced-to-minimum | The triggering input used in the report, compared with the issue's own example. | Passes when the report triggers the behavior with the smallest input that still shows it, rather than a large real-world artifact that buries the signal. Ranks accepted packages only. | preferred |
| separates-adjacent-behavior | Any statement in the report about what still works correctly alongside the failure. | Passes when the report shows a neighbouring case behaving correctly (the format that does still get handled, the input that does still pass), which bounds the bug instead of implying a wholesale breakage. Ranks accepted packages only. | preferred |

## Verdict rule

The verdict is `accept` (ready to post) when **every required check
passes**. Any single required check graded `fail` produces `reject`.

`unclear` on a required check counts as `fail`: a reproduction whose
proof cannot be verified from the package is not proof that is ready to
go upstream.

The one exception is the claim-only draft. When the package is a claim
comment with no repro report yet, the checks whose evidence is the
report — `environment-recorded`, `steps-rerunnable`,
`artifact-shows-behavior`, `conclusion-matches-evidence`,
`reduced-to-minimum`, `separates-adjacent-behavior` — are reported
`unclear` with evidence `not yet applicable: claim-only draft` and are
**excluded from the verdict entirely**, neither passing nor failing it.
The verdict then rests on `claim-names-specifics`,
`claim-promises-only-investigation`, and `repo-conventions-met`, and
answers only: is this claim comment ready to post?

The two preferred checks never change the verdict in either mode.
Report their grades, and use them to separate a package that is merely
sufficient from one that is genuinely easy for a maintainer to act on.
