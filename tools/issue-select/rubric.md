# Rubric: is this a good first issue?

Six required checks, one per way a first contribution dies, plus two
preferred checks that only rank the issues the required set already
accepted.

All recency thresholds are measured against the bundle's stated capture
date in eval mode, and against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-not-archived | The `archived:` field on the repo line of the repo-facts block (live: the archive banner across the top of the repo front page). | Passes when `archived: no`. An archived repo fails: it is read-only, so no pull request can ever merge. | required |
| maintainer-active | The `last 5 default-branch commits` list in the repo-facts block, including each commit's author. | Passes when at least one of the 5 commits is dated within 90 days of the capture date AND is authored by a human. A bot-authored commit counts only when it merges a human's pull request (e.g. a bot landing `#1234`); automated version bumps and dependency updates do not count as maintainer life. | required |
| maintainer-responsive | The `maintainer first-response sample` in the repo-facts block (live: the first Owner/Member/Collaborator reply on a few recently updated issues). | Passes when at least one issue in the sample was opened within 365 days of the capture date AND received a maintainer reply within 90 days of being opened. Issues in the sample with no maintainer comment do not fail the check on their own, as long as one such reply exists. The 365-day qualifier is deliberate: a fast reply in 2021 is evidence about 2021, not about whether anyone is answering the tracker now. | required |
| unclaimed | The `this issue:` line of the repo-facts block (assignees, linked PRs) plus every claim in the comment thread ("I'll take this", "working on this"), with dates. | Passes when there is no assignee, no open linked PR, and no unanswered claim comment newer than 180 days. A closed unmerged PR is an abandoned attempt, not a claim. A claim older than 180 days with no linked PR is stale and does not fail the check; neither does a claim a maintainer has since answered by inviting other contributors. | required |
| bounded-scope | The issue body and the full comment thread. | Passes when the issue asks for one bounded change a newcomer could finish. Fails when it is an umbrella or tracking issue listing sub-items meant to be split, when the thread shows the design still being argued with no maintainer-settled spec, when a maintainer says the fix reaches core internals, or when the issue is a usage/support question rather than a change. A terse body, a missing reproduction, or a bare checklist does not fail this check: grade the size of the work asked for, not the polish of the writing. | required |
| ai-policy-permits | The `contribution policy` line of the repo-facts block (live: `CONTRIBUTING.md` in the repo root or `.github/`, plus `AI_POLICY.md`-style files and PR templates). | Passes unless the policy bans AI-generated contributions outright. Conditions are not bans: disclosure, personal understanding, testing, and human-review requirements all pass, as terms to follow. **Silence passes** — a repo-facts line stating no policy, or making no mention of AI, is not a restriction and must be graded `pass`, not `unclear`. | required |
| newcomer-signposted | The `labels:` on the issue line, and whether the opener's association is OWNER/MEMBER/COLLABORATOR. | Passes when the issue carries a newcomer-facing label (`good first issue`, `help wanted`, `documentation`) or was filed by a maintainer. Ranks only: the label is the maintainer's claim that the work is friendly, never that the issue is free, so it cannot rescue or sink a verdict. | preferred |
| repo-in-use | The `latest release` line and the star count on the repo line of the repo-facts block. | Passes when there is a release dated within 365 days of the capture date, or the repo has at least 100 stars. Preferred on purpose: plenty of healthy projects never cut releases, so this signal must not be able to reject an otherwise live repo. | preferred |

## Verdict rule

The verdict is `accept` when **every required check passes**. Any single
required check graded `fail` produces `reject`.

`unclear` on a required check counts as `fail`: a first issue whose
liveness, scope, or claim state cannot be verified from the evidence is
not a first issue worth taking. The one deliberate exception is written
into `ai-policy-permits` above — an absent AI policy is silence, and
silence passes.

The two preferred checks never change the verdict. Report their grades,
and use them to rank the accepted issues against each other: an accepted
issue that is maintainer-filed or newcomer-labelled, in a repo that is
visibly in use, ranks above an accepted issue that is neither.
