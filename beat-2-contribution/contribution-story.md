# Contribution: docs: add FAQ and troubleshooting guide for CI/CD and Docker environments

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Sehr Abrar
**GitHub Username:** sehr-abrar
**Issue:** https://github.com/Zwony/ecotrace/issues/215
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built the issue-scout, then found that my search strategy — not my rubric — was the part that kept being wrong.**

I installed the scout over my unit-1 issue-select skill and gave it a
search strategy: four `gh search issues` queries traced to sentences in
my fit profile, plus ranking filters that drop assigned or
heavily-commented results and order what survives by fewest comments
first. The scan cost about a dollar and three of the five issues it
paid to grade came from repos created that same afternoon, so I checked
the pool myself instead of guessing why, and found that all 373 issues
my queries returned had been created within the previous 24 hours — `gh`
returns newest-first, so my "drop anything not updated in 90 days"
filter had dropped 0 of 379 and could not possibly fire, and "fewest
comments first" was really sorting by age. My first fix was wrong
twice over: the `--created` windows I added just slid the ceiling
(every result came back pinned to the window's newest edge, spread
zero), and the replacement filter I was proudest of — requiring a gap
between an issue's creation and its last update — turned out to drop
100 of 100 real results, because most issues are never touched again
after filing, including the one I now want to work on. What finally
worked was a search *term* on each query, which shrinks the corpus
instead of repositioning the ceiling: coverage went from one repo and
zero-day median age to 63-85 repos and back-catalogue depth of 1900+
days, and the rule I took away is that a ranking filter which drops 0%
or 100% of a live pool is not a filter — I had written one of each and
could not tell them apart without measuring against real `gh` output.
The rubric needed only two narrow patches, one of which the scout
caught on itself: it rejected my issue because no maintainer had ever
commented on any issue in that repo, which was true, but the maintainer
merges outside contributors' pull requests in 23 minutes, so I taught
`maintainer-responsive` to count a merged outside PR as a reply — as an
additional way to pass, never a new way to fail, so my unit-1 eval
bundles still grade the same.

**Unit 6: Installed the voice-guard and watched two of my own tools disagree about what a claim may promise.**

I folded three drifted copies of my voice guide back into one canonical
file, added the Upstream section with rules for claims, questions and
pings, installed the guard hook, and confirmed it fires: the wiring test
was blocked with "no body the guard can read", which is the pass. Then I
patched my rubric's scope check to the staff `scope-feasible` row, so an
open design edge raises a difficulty note instead of a rejection. The
thing that actually fought me was a contradiction between my own tools:
one of the claim rules I had just written says a claim should carry a
report-back date, and `repro-check` rejected my draft for exactly that,
because its rubric fails any timing commitment at all. repro-check is
right and I was wrong — at claim time I had not read the code, so a
report-back date is as invented as a delivery date, the only difference
being how long it takes everyone to find out. I rewrote the rule to
promise a report-back with no date attached, and kept my original
wording in it as a second Wrong example, because a guide that tells me
to write what my own checker rejects is worse than no guide. repro-check
then caught three real overstatements in my reproduction report —
including a claim that the troubleshooting page "answers none of" the
issue's topics, when it already answers a neighbouring one for
Windows/macOS — each of which I verified against the file before
accepting the correction.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

This issue came from the AI301 Issue Selection site. I filtered it the
way my fit profile reads — Python, documentation — and `Zwony/ecotrace`
came back: a Python library that measures the energy and carbon cost of
running code. Issue #215 asks for a `docs/FAQ.md` answering four
deployment questions about Docker, RAPL counters, headless CI runs and
log noise, plus a nav entry in `mkdocs.yml`.

I graded two issues with my scout. The first was #183, an exporter
feature in the same repo, and my scout rejected it on
`maintainer-responsive`: no maintainer had commented on any issue in the
repo, across forty of them. I checked that myself rather than taking the
verdict, and found the check was reading the wrong surface — the
maintainer merges outside contributors' pull requests in 23 minutes, 4.5
hours and 14 hours, and simply never comments on issues. The repo's
`CONTRIBUTING.MD` explains why: it points contributors at a Discord for
"advice, pairing, or a fast code review". So the signal my check was
looking for lives somewhere my check could not see. I patched
`maintainer-responsive` to count a merged outside pull request as a
response, written as an additional way to pass rather than a new way to
fail, so my unit-1 eval bundles still grade identically. #215 then
passed every required check, and after this week's scope patch it came
back `accept` with the difficulty note **straightforward**.

I chose #215 over #183 for a reason that only showed up when I read the
code. #183 tells you to follow the conventions in `ecotrace/exporters/`
— but that package holds `CloudExporter`, `OTelExporter` and
`WebhookExporter`, which are push-to-platform network clients, and the
CSV/JSON/PDF formatters the issue describes are not in it at all; the
PDF one lives in `ecotrace/report.py`. So the issue asks for free
functions, points at class-based conventions that do not fit them, and
names a test file that tests something else. That is a design question
only the maintainer can settle, and this is the maintainer who never
answers issues. #215's hard part is different: it is reading. The spec
is closed — one new file, one nav line, an explicit instruction not to
touch `ecotrace/` — and everything I need to be accurate is in the
repository, where I can verify it. I would rather spend two weeks on
work I can check than on a guess I cannot.

What I want out of it is the workflow, not the difficulty. This is my
first real contribution, and I want the claiming, reproducing, pull
request and review loop to be the thing I learn, on a change small
enough that the mechanics stay visible. The honest risk is that
documentation is only cheap if it is correct: I have to understand how
EcoTrace actually behaves when `/sys/class/powercap` is unreadable
before I can write a sentence about it, and a confidently wrong FAQ is
worse than no FAQ.

My honest read on the scout is that it has been right about repos and
wrong about search. Its rubric has needed two narrow patches in three
weeks and both were real false negatives I could evidence. My search
strategy, by contrast, was wrong three times, and twice I recorded a fix
that I had only verified enough to confirm what I already believed —
once shipping a filter that dropped 0 of 379 results, then replacing it
with one that dropped 100 of 100. What actually caught both was running
the query myself and looking at the distribution of what came back,
rather than reading the scan's prose. The tool is useful exactly to the
degree that I check it.

**Claim comment:** claim pending (as of 2026-11-01) — draft written and
passing both my voice guard and `repro-check`; link goes here the moment
it is posted.

**Toolkit**

- Tool: issue-scout — my search strategy and my rubric. I ran one live
  scan ($1, five issues graded), then graded #183 and #215 directly, and
  re-graded #215 this week after patching the rubric's scope check.
- What it produced: `reject` on #183 (`maintainer-responsive`), and
  `accept` on #215 with every required and both preferred checks
  passing, carrying the difficulty note **straightforward**. The read-out
  also surfaced the thing that decided my pick: #215 names its two files
  and forbids touching `ecotrace/`.
- Difficulties: two false negatives and one near-miss. `maintainer-active`
  passed a repo with a single commit of total history. `maintainer-responsive`
  rejected #215's repo for having no issue comments, when the maintainer
  merges outside PRs in under an hour. And this week's `bounded-scope`
  check would have failed every issue in that repo on a technicality —
  it demanded a "maintainer-settled spec", and a maintainer who never
  comments can never settle one.
- What changed: `maintainer-active` now fails a default branch with
  fewer than five commits of history. `maintainer-responsive` now counts
  a merged outside pull request as a response. `bounded-scope` is gone,
  replaced by the staff `scope-feasible` row plus a difficulty-note
  section, so an open design edge raises a flag instead of a veto —
  which is right, because #215's open edge (whether the RAPL-versus-TDP
  answer belongs in the FAQ or links out to the existing benchmark page)
  is now the first question in my claim rather than a reason my own tool
  rejects the issue.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- macOS 15.6 (Darwin 24.6.0), Apple Silicon
- Python 3.12.3
- git 2.39.5 (Apple Git-154)
- EcoTrace `1.8.0` (`ecotrace/__init__.py`: `__version__ = "1.8.0"`)
- Commit under test: `695ad44` — "release: v1.8.0 community and
  hardware expansion release (#246)", 2026-11-01

Setup was unusually light, and that is a property of this issue rather
than luck. #215 is a documentation gap, so every check reads the
repository's own sources: `git clone https://github.com/Zwony/ecotrace.git`
and `git checkout 695ad44` is the whole environment. No `pip install -e
".[dev]"`, no `pytest` run, no Docker. One challenge did come up and it
was mine: I first cloned with `--depth 1`, which reproduces fine today
but cannot check out `695ad44` once `main` moves on, so the steps below
specify a full clone instead.

**Steps to Reproduce**

1. Clone the repo and `git checkout 695ad44`.
2. Confirm the page the issue asks for does not exist, and that nothing
   in the documentation refers to it:
   `ls docs/FAQ.md` → `No such file or directory`;
   `grep -in "faq" mkdocs.yml` → no output;
   `grep -ril "faq" --include="*.md" --include="*.yml" .` → no output.
3. Open the page a stuck user would actually reach. `docs/SUPPORT.md`
   is in the `mkdocs.yml` nav under Community and carries a
   `## Troubleshooting` heading.
4. Count the issue's four topics on that page:
   `for t in powercap Docker RAPL headless "GitHub Actions" verbose; do printf '%-16s -> ' "$t"; grep -ci "$t" docs/SUPPORT.md; done`
5. Observed result: `powercap 0`, `Docker 0`, `RAPL 2`, `headless 0`,
   `GitHub Actions 0`, `verbose 0`. The page a user opens when a
   deployment breaks mentions none of containers, powercap, headless
   operation, GitHub Actions, or log control.

**Reproduction Evidence**

- **Commit showing reproduction:** none. This is a documentation gap
  reproduced by reading the repository at `695ad44`, so there is
  nothing to commit to a fork to demonstrate it; the commands above are
  the artifact and they re-run on a clean clone.
- **Screenshots/logs:**

  ```
  $ ls docs/FAQ.md
  ls: docs/FAQ.md: No such file or directory

  $ grep -n -i rapl docs/SUPPORT.md
  14:**RAPL Not Available (Windows / macOS)**
  15:EcoTrace automatically selects the Boavizta advanced estimation model when RAPL hardware counters are unavailable.

  $ grep -rn -iE "headless|GITHUB_ACTIONS" --include="*.md" docs/
  docs/history/releases/RELEASE_v1.5.0.md:27:Authenticate your terminal once to stream all headless profiling runs directly to your web dashboard:
  ```

- **My findings:** the gap is real but narrower than the issue implies,
  and saying so precisely is the useful part. `docs/SUPPORT.md:14-15`
  already answers a neighbouring question — when RAPL counters are
  unavailable, EcoTrace falls back to the Boavizta estimation model —
  but it is scoped to **Windows and macOS**, where RAPL does not exist.
  The issue asks about Linux containers, where RAPL exists and is
  *withheld*: `/sys/class/powercap` unmounted, or readable only with
  elevated permissions. That case is not covered. The material that
  bears on it is scattered across `docs/ARCHITECTURE.md:67-68` (a design
  description of the two measurement modes) and
  `docs/benchmarks/07_accuracy_validation.md` (the right content, filed
  under Academic Benchmarks, which is not where someone debugging a
  container looks). For the headless-CI question there is no answer in
  the documentation at all: the single docs hit is a v1.5.0 release note
  about `ecotrace login` streaming to a dashboard, which is a different
  subject. What I have **not** done is run EcoTrace in a container or in
  GitHub Actions, so I have shown that the docs do not describe the
  behavior, not yet what the behavior is. Establishing that is the first
  half of writing the page.

**Toolkit**

- Tool: repro-check, run twice on the claim alone and three times on
  the claim and reproduction report together, plus the voice-guard hook
  on the claim draft.
- What it produced: `reject`, `reject`, then `accept`. Each reject named
  one check and quoted the line that failed it, which made them cheap to
  act on.
- Difficulties: the two rejects on the report were both me overstating
  evidence I had not shown. I wrote that `SUPPORT.md`'s RAPL hits were
  "in the hardware support tables" when they are a Troubleshooting
  entry, and that "faq" appears "nowhere in the repository" when my grep
  had only searched `.md` and `.yml` (it is in a lockfile). Both read as
  confident facts and neither was backed by output I had pasted — which
  is my own voice-guide rule, "Report what I ran, label what I think",
  broken in my own report. A separate annoyance: repro-check reported it
  could not find my voice guide, because the install symlinks it and
  that subprocess would not follow the link; the guard itself reads it
  fine, so the rules were still enforced.
- What changed: I added the `grep -n -i rapl` and headless greps to the
  report so each claim carries its own output, narrowed "nowhere in the
  repository" to "no `.md` or `.yml` file", and rewrote the gap
  statement to concede what `SUPPORT.md` already covers before saying
  what it does not. The report got shorter in claims and longer in
  evidence, which is the right direction.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
