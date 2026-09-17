# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

**Verdict output**

Live-mode run graded three candidates (#53, #72, #68); full output below. The
selected issue is #53, verdict `accept`.

```
Skill(issue-select)
  Successfully loaded skill

Read 3 files
Now let me gather evidence for all three issues and the repo in parallel.

Bash(gh repo view codepath/pathreview-ai301-fa26-s3 --json isArchived,stargazerCount,latestRelease,description 2>&1)
  Error: Exit code 4
  To get started with GitHub CLI, please run:  gh auth login
  Alternatively, populate the GH_TOKEN environment variable with a GitHub API authentication token.

gh isn't authenticated. I'll fetch via the web instead.

Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3)
  Received 297KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53)
  Received 243.4KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72)
  Received 240.2KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68)
  Received 240.6KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/commits)
  Received 481.8KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues)
  Received 354KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/find/main)
  Received 183KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues?q=is%3Aissue+is%3Aclosed)
  Received 248.9KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues?page=3)
  Received 355KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues/52)
  Received 239.8KB (200 OK)
Fetch(https://github.com/codepath/pathreview-ai301-fa26-s3/issues/1)
  Received 239.9KB (200 OK)

I now have all the evidence I need. Let me compile the grades.

---
Issue-by-Issue Evidence Summary

Repo-level checks (apply to all three)
- repo-not-archived: isArchived: false -> pass
- maintainer-active: Andrew Burke (human) committed on Sep 16, 2026 -- within 90 days -> pass
- maintainer-responsive: Issue #52 (opened Sep 10, 2026) received a Collaborator reply from @Aburke225 on Sep 16, 2026 -- within 90 days of opening, within 365 days of today -> pass
- ai-policy-permits: No CONTRIBUTING.md found (404); silence passes -> pass

---
Issue #53 -- PII scrubber fails to redact parenthesized US phone numbers

| Check | Grade | Evidence |
|---|---|---|
| repo-not-archived | pass | isArchived: false |
| maintainer-active | pass | Andrew Burke committed Sep 16, 2026 (today, <90 days) |
| maintainer-responsive | pass | #52: Collaborator @Aburke225 replied 6 days after opening |
| unclaimed | pass | No assignees, no linked PRs, no claim comments |
| bounded-scope | pass | Single regex fix in pii_scrubber.py; steps to reproduce and 4 failing tests named |
| ai-policy-permits | pass | No CONTRIBUTING.md -- silence passes |
| newcomer-signposted (preferred) | pass | Labels: bug, good first issue, safety, tier-1; opener is Collaborator |
| repo-in-use (preferred) | fail | No releases, 1 star -- below both thresholds |

Verdict: ACCEPT

---
Issue #72 -- verify_password raises UnknownHashError on malformed stored hashes

| Check | Grade | Evidence |
|---|---|---|
| repo-not-archived | pass | isArchived: false |
| maintainer-active | pass | Andrew Burke committed Sep 16, 2026 (today, <90 days) |
| maintainer-responsive | pass | #52: Collaborator @Aburke225 replied 6 days after opening |
| unclaimed | pass | No assignees, no linked PRs, no claim comments |
| bounded-scope | pass | One try/except fix in core/security.py; xfail test to unmask; 1-2 hr estimate |
| ai-policy-permits | pass | No CONTRIBUTING.md -- silence passes |
| newcomer-signposted (preferred) | pass | Labels: api, bug, good first issue, tier-1; opener is Collaborator |
| repo-in-use (preferred) | fail | No releases, 1 star -- below both thresholds |

Verdict: ACCEPT

---
Issue #68 -- Keyword search raises ZeroDivisionError when the index is empty

| Check | Grade | Evidence |
|---|---|---|
| repo-not-archived | pass | isArchived: false |
| maintainer-active | pass | Andrew Burke committed Sep 16, 2026 (today, <90 days) |
| maintainer-responsive | pass | #52: Collaborator @Aburke225 replied 6 days after opening |
| unclaimed | pass | No assignees, no linked PRs, no claim comments |
| bounded-scope | pass | Single guard clause in rag/retriever/keyword_search.py; xfail test to unmask; 2-4 hr estimate |
| ai-policy-permits | pass | No CONTRIBUTING.md -- silence passes |
| newcomer-signposted (preferred) | pass | Labels: bug, good first issue, rag, tier-1; opener is Collaborator |
| repo-in-use (preferred) | fail | No releases, 1 star -- below both thresholds |

Verdict: ACCEPT

---
Ranking (fit profile)

All three accepted. Ranked by fit: Python bug fixes are highest priority; smallest
scope and most concrete reproduction steps go first.

1. #53 -- Regex fix in pii_scrubber.py. Most self-contained: one pattern change,
   exact failing input shown in the body, four named failing tests. The entire fix
   is a regular-expression edit -- the workflow is the hard part, exactly what you want.
2. #72 -- Exception-handling fix in core/security.py. Slightly more involved (need
   to understand passlib's UnknownHashError) but still a tight try/except wrap;
   1-2 hr estimate.
3. #68 -- Empty-corpus guard in rag/retriever/keyword_search.py. Correct and doable,
   but the 2-4 hr estimate and the BM25 context make it marginally more to absorb
   than #53 or #72.

---
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false on repo metadata"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "Andrew Burke (human) committed Sep 16 2026, within 90 days"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Issue #52: Collaborator @Aburke225 replied Sep 16 2026, 6 days after Sep 10 opening"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no linked PRs, no claim comments in thread"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single regex fix in pii_scrubber.py; reproduction steps and 4 named failing tests provided"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "No CONTRIBUTING.md found (404); silence passes per rubric"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels: good first issue, bug, safety, tier-1; opener @Aburke225 is Collaborator"},
      {"name": "repo-in-use", "grade": "fail", "evidence": "No releases (releaseCount: 0); 1 star -- below both thresholds"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false on repo metadata"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "Andrew Burke (human) committed Sep 16 2026, within 90 days"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Issue #52: Collaborator @Aburke225 replied Sep 16 2026, 6 days after Sep 10 opening"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no linked PRs, no claim comments in thread"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One try/except wrap in core/security.py to catch UnknownHashError; xfail test to unmask; 1-2 hr estimate"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "No CONTRIBUTING.md found (404); silence passes per rubric"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels: good first issue, bug, api, tier-1; opener @Aburke225 is Collaborator"},
      {"name": "repo-in-use", "grade": "fail", "evidence": "No releases (releaseCount: 0); 1 star -- below both thresholds"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false on repo metadata"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "Andrew Burke (human) committed Sep 16 2026, within 90 days"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Issue #52: Collaborator @Aburke225 replied Sep 16 2026, 6 days after Sep 10 opening"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no linked PRs, no claim comments in thread"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single empty-corpus guard in rag/retriever/keyword_search.py; xfail test to unmask; 2-4 hr estimate"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "No CONTRIBUTING.md found (404); silence passes per rubric"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels: good first issue, bug, rag, tier-1; opener @Aburke225 is Collaborator"},
      {"name": "repo-in-use", "grade": "fail", "evidence": "No releases (releaseCount: 0); 1 star -- below both thresholds"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Before any scored run, I hand-graded the four calibration bundles against my draft
rubric at no cost. That produced no agreement score, but it did surface one real
defect: `maintainer-responsive` passed `calib-04` (sharkdp/bat) on maintainer replies
from 2021 and 2023, while all three of the repo's 2026 issues had no maintainer comment
at all. The check was measuring how *fast* a reply came, not how *recently*. I added the
365-day recency qualifier before running anything.

An earlier attempt to launch the harness aborted with all 20 items erroring before any
model call (the harness shells out to `claude`, and I first ran it from inside a Claude
Code session, which refuses to nest). It produced no agreement score, nothing was
billed, and `--save-run` correctly refused to write a partial run. I re-ran it from a
plain terminal.

Scored runs, in order:

1. **18/20 scored items, bar: PASS.** Categories: claimed 4/4, clear-accept 6/8,
   dead-repo 3/3, policy 1/1, scope 4/4. Category floor met.

This is the run recorded in `eval-run.txt`, and 18/20 is its agreement line. I stopped
here deliberately rather than chasing 20/20: the bar was met with the category floor
intact, and the two remaining disagreements are both real limitations of my checks that
I would rather describe honestly below than paper over with a threshold tuned to the
answer key.

**Issue analysis**

**`issue-14`** (`LegalQuants/lq-ai#490`). **My rubric: `reject`. Gold label: `accept`.**

Seven of my eight checks passed it, and the issue is an obvious first contribution: a
documentation-only change removing Discord references from five named files, labelled
`documentation` and `good first issue`, no assignee, no linked PRs, no comments. The
repo is plainly alive: five commits in the five days before capture, by two different
human authors, and a release dated 2026-07-04.

It was rejected on one check, `maintainer-responsive`, and the evidence line my skill
recorded explains why:

> Sample has one issue (#489, opened 2026-08-04) with no maintainer comment; pass
> condition requires at least one qualifying reply, none exists

The repo-facts block advertises a sample of "5 recently updated issues," but this repo
only had one to sample, and that one issue was opened **a single day before the capture
date**. My check requires a qualifying reply to *exist* in the sample, so a sample too
thin and too fresh to contain one fails automatically.

The reasoning error is that my check treats *absence of evidence* as *evidence of
absence*. "No maintainer has replied to a one-day-old issue" is not the same fact as
"this project does not answer its tracker," but my pass condition cannot tell them
apart. Worth noting: the 365-day recency qualifier I added after calibration did not
cause this. Issue #489 has no maintainer comment at all, so it failed under the original
wording too. This is a pre-existing blind spot the calibration set never exposed,
because all four calibration repos had sample entries with actual replies in them.

**Check rationale**

The check, quoted as it currently stands in `tools/issue-select/rubric.md`:

> | maintainer-responsive | The `maintainer first-response sample` in the repo-facts
> block (live: the first Owner/Member/Collaborator reply on a few recently updated
> issues). | Passes when at least one issue in the sample was opened within 365 days of
> the capture date AND received a maintainer reply within 90 days of being opened.
> Issues in the sample with no maintainer comment do not fail the check on their own, as
> long as one such reply exists. The 365-day qualifier is deliberate: a fast reply in
> 2021 is evidence about 2021, not about whether anyone is answering the tracker now. |
> required |

Three decisions are encoded here, each answering a specific failure I saw.

*Only one reply is needed.* Requiring most sampled issues to get a response would reject
large, healthy projects as a matter of course. `issue-01` (conda/conda) has four of five
sampled issues with no maintainer comment and is a correct `accept`; a majority-response
threshold would have thrown it away. What this check is really built to catch is the
repo where *nobody answers anything*, which is `calib-03` (httpie/cli): one sampled
issue, open since 2023, never answered, with five open PRs stacked up behind it.

*90 days to reply.* A quarter is slow but survivable for a volunteer project. A 30-day
threshold would punish maintainers who batch their tracker work; a year is so loose the
check stops discriminating.

*Opened within 365 days of capture.* This is the qualifier calibration forced. Without
it the check reads any reply in the sample, however old, as proof of current life,
which is precisely how `calib-04` (sharkdp/bat) passed on 2021 evidence while ignoring
that its three 2026 issues went unanswered.

**Trade-offs**

This check has a demonstrated cost, and `issue-14` is the issue whose result it changes.

Making a maintainer reply *required* means the check fails whenever the sample is too
small or too new to contain one, and it cannot distinguish that from genuine silence.
`issue-14` is the case: a live repo with a week of human commits, rejected because its
only sampled issue was one day old. That is my single clear-accept miss, and it is the
direct price of this check's shape.

The fix would be to grade `unclear` instead of `fail` when the sample holds fewer than
some number of entries, or to let `maintainer-active` carry the verdict when the commit
history is unambiguous. I did not make that change, for two reasons. The looser rule
would also stop `calib-03` from being caught on this check, and more importantly the
alternative is not obviously right: a project with one issue in its tracker and no reply
on it genuinely *is* an unknown quantity for a newcomer, and my verdict rule's stated
position is that an unverifiable first issue is not one to take.

So I am accepting a known false reject on thin-sample repos in exchange for catching
silent-tracker repos, and `issue-19` shows the same pattern in `bounded-scope`: a
maintainer-diagnosed performance bug listing two root causes and three optimization
suggestions read to my check as an umbrella of sub-items, when they are really several
proposed routes to fixing one symptom. Both misses are the same species: a required
check that cannot tell "several things to do" from "one thing described in several
parts."

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

I picked #53 because it's a single, well-defined regex fix with reproduction steps
and failing tests already in hand, exactly the kind of small, complete unit I want
for learning the contribution workflow itself rather than fighting unfamiliar code.
It's also PII redaction, which is close to what I actually deal with at my job in
health insurance, so I have some intuition for why the fix matters.

The rubric correctly confirmed the mechanical facts: active maintainer, unclaimed,
small bounded scope, no AI-use restriction. What it can't weigh is fit, that this
happens to sit in a domain I already understand, and that starting with "change one
regex, watch four tests turn green" is the right size of first win for someone who's
barely touched open source before.

I expect claiming to be low-friction, since it's an unclaimed, well-scoped bug with a
clear repro in a classroom repo, but actually fixing it may take some care getting the
phone-number regex right without breaking the formats it already catches.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
