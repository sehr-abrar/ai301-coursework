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

**Postscript, the same day I claimed.** The maintainer replied in 31
minutes — the first issue comment he has made in at least forty issues.
He assigned the issue to me ("The issue is all yours") and answered my
open question directly: a concise FAQ linking out to the benchmark page
"is the ideal approach."

That reply is the most interesting thing my scout got wrong, and it is
worth being precise about what the mistake was. My rubric rejected this
repo for having no issue replies, and I patched `maintainer-responsive`
to count merged outside pull requests instead, on the theory that this
maintainer does his talking in the PR tab. The patch was useful and the
verdict it produced was right — but the premise under it was wrong.
He is not a maintainer who refuses to answer issues. He is a maintainer
nobody had asked anything: all forty of those issues were filed by him,
and not one carried a question. The first person to ask one got an
answer in half an hour. What looked like a property of the maintainer
was a property of the thread.

So the check I patched was measuring the right thing badly, and the
check I would now add is different again: not "does anyone reply here"
but "has anyone given them something to reply to". I cannot grade that
from issue metadata, which is probably why I did not see it.

**Claim comment:** https://github.com/Zwony/ecotrace/issues/215#issuecomment-6563399356
(posted 2026-11-01). I drafted it, failed my own `repro-check` once for
promising a report-back date, cut the date, then reworded the opening
and closing into my own voice before posting. I re-ran the voice guard
against the text I actually sent rather than the text I had checked
earlier, and it passed.

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

EcoTrace measures the energy and carbon cost of running code. On Linux
it prefers real hardware energy counters, but in a container or a CI
runner those counters are usually unreadable, and EcoTrace quietly
switches to estimating instead. Nothing tells the user that happened.
There is no page in the documentation that explains the switch, what it
costs in accuracy, how to run EcoTrace headlessly, or how to quiet its
logs — so a user whose numbers look wrong in Docker has nowhere to look.

**Expected Behavior**

A reader with a deployment problem should be able to open one page and
find out why their numbers changed, how far off an estimate can be, what
to set for a headless CI run, and how to silence the logger. The issue
asks for that page specifically: `docs/FAQ.md`, listed in the `mkdocs.yml`
navigation, covering four named topics.

**Current Behavior**

`docs/FAQ.md` does not exist, and the string "faq" appears in no `.md`
or `.yml` file in the repository. The page a stuck user would actually
open, `docs/SUPPORT.md`, has a `## Troubleshooting` section that mentions
Docker, powercap, headless operation and verbose logging exactly zero
times. Its one neighbouring entry, "RAPL Not Available (Windows /
macOS)", covers platforms where the counters do not exist — not Linux
containers, where they exist but are withheld. The partial material that
does bear on the question is scattered across a design document, a
benchmark page filed under Academic Benchmarks, and a historical release
note.

**Affected Components**

`docs/FAQ.md` (new) and `mkdocs.yml` (one nav line) are the only files
changed. The behaviour being described lives in `ecotrace/hardware.py`
(`_check_rapl`, `get_cpu_energy_j`, `estimate_cpu_power_w`),
`ecotrace/cpu.py` (`get_cpu_info`, the TDP lookup and its defaults),
`ecotrace/logger.py` (the logger's name and level), and
`ecotrace/ml.py` (the module-level import that pulls in the charting
stack). None of those files is modified: the issue forbids it, and
reading them is the work.

**Toolkit**

- Tool: repro-check (unit 2) for the reproduction, and my own reading of
  the four modules above to establish what the FAQ would have to say.
- What it produced: a reproduction report that passed on the third
  attempt, and a set of verified facts — the single powercap path, the
  Boavizta load curve, the 25 W / 65 W TDP defaults, the eager import in
  `ml.py` — that each answer could be built on.
- Difficulties: the tool graded my writing, not my understanding, and
  twice caught me asserting something I had not shown. It had nothing to
  say about whether my description of the code was *correct*; only
  running the interpreter settled that.
- What changed: I stopped writing a claim before I had the output that
  backed it, and I started pasting the command next to the sentence it
  supports. The reproduction report got shorter in claims and longer in
  evidence.

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

The cause is not a code fault. It is that EcoTrace's behaviour in
containers is *deliberately silent*, and the documentation never says so.

- `HardwareMonitor._check_rapl()` (`ecotrace/hardware.py:20`) runs only
  on Linux and probes exactly one path,
  `/sys/class/powercap/intel-rapl/intel-rapl:0/energy_uj`. On a missing
  file or a `PermissionError`/`IOError` it returns `False` and logs at
  **debug** level only. The comment on that line says why: "Fail
  silently to preserve the zero-config experience."
- What replaces it: `get_cpu_energy_j()` (`hardware.py:91`) falls
  through to `estimate_cpu_power_w()` (`hardware.py:116`), a piecewise
  Boavizta curve mapping load to a share of rated TDP (0%→12%, 10%→32%,
  50%→75%, 100%→102%). The TDP is looked up in `cpu.py:get_cpu_info()`
  and **defaults to 25 W on Apple silicon and 65 W otherwise** when the
  CPU is not in the dataset.
- Accuracy is already measured but filed under Academic Benchmarks, and
  the page carries two different numbers: a measured MAPE of 40.3% and
  R² 0.7181 against a synthetic reference, and a separate prose claim of
  "typical" MAPE ≤ 15% and R² > 0.90 for runs with real RAPL access.
- `ecotrace/logger.py` is a plain stdlib logger named `ecotrace` at
  `WARNING`. There is no EcoTrace-specific setting for verbosity.
- The headless answer is the opposite of what the code first suggests.
  `core.py:1239` imports the reporting module lazily, which looks like
  the charting stack is optional — but `ecotrace/ml.py:10` imports it at
  **module level** and `__init__.py` imports `ml`, so a bare
  `import ecotrace` always loads `matplotlib.pyplot` and `fpdf`.

**Proposed Solution**

Create `docs/FAQ.md` with four sections, one per question in the issue,
each traced to the code above rather than to general advice, and add one
`- FAQ: FAQ.md` entry to the Community block of `mkdocs.yml` after
Support. The RAPL-versus-TDP answer links out to the existing accuracy
benchmark rather than restating its figures, which was the open question
I raised in my claim and the default I said I would take.

Deliberately not changing: anything under `ecotrace/` (the issue forbids
it, and the eager `ml.py` import is arguably a bug but is not this
issue); `docs/SUPPORT.md`, which the FAQ links to instead of rewriting;
`docs/requirements.txt`; and the 759 pre-existing `flake8` findings and
60 `griffe` strict-mode warnings already failing on `main`.

**Implementation Plan**

1. Branch `docs/faq-troubleshooting` from `main` at `695ad44`, the
   branch name the issue's own steps give.
2. Write `docs/FAQ.md`, four `##` sections, with file and function
   references wherever a claim comes from the code.
3. Add `- FAQ: FAQ.md` to the Community block of `mkdocs.yml`.
4. Verify `mkdocs build` exits 0 with no missing-nav warning and renders
   `site/FAQ/index.html`.
5. Verify `pytest` still reports 251 passed / 2 skipped and the diff
   touches exactly two files.
6. Commit in the repo's conventional-commit style.

*Deviations, recorded as they happened:*

- **Plan revised before building, twice, after `plan-check` rejected
  it.** My first draft cited the benchmark page's "MAPE ≤ 15%" as if it
  were the measured result. It is not — the page's own run measured
  40.3% — and I had quoted the flattering number. The plan and the FAQ
  now give both figures, each labelled with what produced it.
- **The Docker fix was downgraded from an instruction to a diagnosis.**
  I had planned to tell readers to bind-mount `/sys/class/powercap`
  read-only. I cannot test that: this machine is arm64, where Intel RAPL
  cannot exist, and Docker's daemon was not running either. Rather than
  publish an untested `docker run` flag, the FAQ now states the two
  conditions that must hold and gives the reader a one-line `cat` to
  test their own environment.
- **Branch name corrected** from `docs/215-faq-troubleshooting` to
  `docs/faq-troubleshooting`, which is what the issue actually specifies.
- **One wording change after writing the evidence.** My grep table showed
  `verbose -> 0` in the new FAQ: I had answered the logging question
  without ever using the issue's own word, so nobody searching "verbose"
  would land there. I reworded that section's heading and opening.

**Toolkit**

- Tool: plan-check, run twice over `plan.md` and a draft `plan-comment.md`.
- What it produced: `reject`, then `accept`. The reject named one check,
  `honesty-calibrated`, and quoted both offences — the accuracy figure
  and the untested bind mount.
- Difficulties: it caught a failure mode I did not know I had. Both
  problems were me reaching for the more convenient version of a fact: a
  better-sounding accuracy number, and a fix stated as settled when I had
  no way to run it. Neither was a careless error; both read as confident
  and were wrong. It also flagged that its own `procedure.md` assumes a
  posted repro comment exists, which is never true for a docs issue.
- What changed: `plan.md` gained a "Risks and unknowns" section naming
  what I cannot verify on arm64 hardware, and the FAQ's Docker answer
  became a diagnosis rather than an instruction. I also patched
  `plan-check/procedure.md` so it states how to grade a wild issue with
  no posted repro — the evidence is whatever the drafts quote from the
  repository, verified at the stated commit.

---

## Testing Strategy

Three layers, because a documentation change can be "built" and still
be wrong in a way no build catches.

**1. The reproduction, re-run against the branch.** The same greps that
established the gap now run on the fix:

| Topic | `docs/SUPPORT.md` on `main` | `docs/FAQ.md` on the branch |
|---|---|---|
| powercap | 0 | 6 |
| Docker | 0 | 1 |
| RAPL | 2 | 15 |
| headless | 0 | 2 |
| GitHub Actions | 0 | 1 |
| verbose | 0 | 2 |

`ls docs/FAQ.md` goes from `No such file or directory` to the file, and
`grep -n "FAQ" mkdocs.yml` from no output to `85: - FAQ: FAQ.md`.

**2. The project's own checks, from my ship manifest.**

- `.venv/bin/python -m pytest tests/ -q` → **251 passed, 2 skipped**,
  identical to the `main` baseline. The change adds no Python, so no
  movement was the expected result and is what happened.
- `.venv/bin/mkdocs build` → **exit 0**, zero "not found in the
  documentation files" warnings, `site/FAQ/index.html` rendered, and the
  FAQ link present in the Community nav of sibling pages. This is the
  check that actually exercises my change: a nav entry pointing at a
  file that does not exist produces that warning, and a broken docs
  build would break the published site.
- `git diff --stat main..HEAD` → exactly two files, 187 insertions, zero
  deletions.

Two checks are excluded in the manifest with dated reasons: `flake8`
already fails on `main` with 759 findings, and `mkdocs build --strict`
aborts on `main` with 60 `griffe` warnings raised from docstrings in
`ecotrace/` — a tree this PR does not touch.

**3. Every factual claim on the page, executed.** This is the layer that
matters most for a docs change, because prose cannot fail a build. Each
behavioural statement in the FAQ was run before it was written:

```
$ python -c "import sys, ecotrace; print('matplotlib' in sys.modules)"
True
$ python -c "from ecotrace.hardware import HardwareMonitor; print(HardwareMonitor().rapl_available)"
False
$ python -c "import logging, ecotrace; print(logging.getLevelName(logging.getLogger('ecotrace').level))"
WARNING
$ MPLBACKEND=Agg python -c "import matplotlib, ecotrace; print(matplotlib.get_backend())"
Agg
```

**What this strategy does not prove.** This host is arm64, so
`/sys/class/powercap` cannot exist and the container guidance was never
run against a RAPL-equipped Linux host. That is why the FAQ states the
two conditions that must hold and hands the reader a `cat` command to
test their own environment, instead of presenting a `docker run`
invocation I could not execute. The `rapl_available` False above shows
the silent-fallback path, not the container case specifically.

**Toolkit**

- Tool: the ship-gate, running my manifest's checks and then pr-precheck
  over the branch, plan, evidence and PR description.
- What it produced: see the Pull Request section's Toolkit block for the
  gate's verdicts; the manifest itself is what turned "run the tests"
  into two named commands with a measured baseline.
- Difficulties: writing the manifest forced a decision I had been
  avoiding — `flake8` and `mkdocs --strict` both fail on `main`, so
  either they gate nothing or they gate on noise. Putting them in the
  excluded table with dated reasons was the honest answer, but it means
  my must-pass set is only two checks deep.
- What changed: I added `mkdocs build` as a must-pass check, which was
  not in `CONTRIBUTING.MD` at all. The repo names `pytest` and `flake8`,
  neither of which can detect a broken docs nav — the one way this
  particular change could fail.

---

## Implementation Notes

A running log of what I built and the turns I got wrong.

**2026-11-01**

Claimed #215 on the issue thread after `repro-check` rejected my first
draft for promising a report-back date. Reproduced the gap at `695ad44`:
`docs/FAQ.md` absent, no "faq" in any `.md` or `.yml`, and
`docs/SUPPORT.md` scoring zero on five of the issue's six keywords. Two
`repro-check` rejects after that, both for the same fault — writing a
confident sentence without pasting the output behind it. I had claimed
`SUPPORT.md`'s RAPL hits were "in the hardware support tables"; they are
a Troubleshooting entry, and reading them instead of counting them is
what showed the gap is narrower than the issue implies.

**2026-11-08**

Installed the ship-gate against `~/Desktop/ecotrace` and wrote the
manifest. Measured a baseline on `main` before touching anything, which
is what surfaced that `flake8` has 759 pre-existing findings and
`mkdocs build --strict` aborts on 60 `griffe` warnings. Both went to the
excluded table with reasons rather than being quietly dropped.

Orientation on the subsystem gave me a wrong answer that I nearly
shipped. `core.py:1239` imports the reporting module lazily, so I
concluded `import ecotrace` leaves `matplotlib` alone and that headless
runs are free of the charting stack. Checking `sys.modules` after a bare
import showed `matplotlib` and `fpdf` both present: `ml.py:10` imports
the reporting module at module level and `__init__.py` imports `ml`, so
the lazy import in `core.py` is defeated. `-X importtime` puts ~0.46 s
of a ~0.53 s import on that chain. Had I trusted the read instead of
running it, the FAQ would have told people the opposite of the truth.

Second wrong turn, caught by `plan-check`: I quoted the accuracy
benchmark's "MAPE ≤ 15%" as the measured figure. The page's own run
measured **40.3%**; the ≤ 15% is unsourced prose about Linux hosts with
real RAPL. I had reached for the flattering number without noticing. The
FAQ now carries both, each labelled with what produced it.

Third: I planned to tell readers to bind-mount `/sys/class/powercap`
read-only. This machine is arm64 — Intel RAPL cannot exist here, and
Docker's daemon was not running — so I had no way to test it, and
`_check_rapl()` catches `PermissionError` as well as a missing file, so
exposing the path may not be sufficient for a non-root container user.
The Docker answer became a diagnosis the reader can run themselves
rather than an instruction I could not stand behind.

Built the branch, verified `mkdocs build` renders the page into the
Community nav, confirmed `pytest` unchanged at 251 passed / 2 skipped.
Last catch came from my own evidence table: the FAQ scored `verbose → 0`
because I had answered the logging question without ever using the
issue's word for it. Reworded so a reader searching "verbose" lands on
the right section.

Then the maintainer answered my claim, 31 minutes after I posted it. He
confirmed the issue is mine and endorsed the structure I had already
built: "a concise explanation in FAQ.md that links directly to
`benchmarks/07_accuracy_validation.md` for in-depth details is the ideal
approach." My plan had carried "the link-out decision is still
unanswered" as an open risk, on the grounds that he had never replied to
an issue; that risk is now closed by his own words rather than by my
assumption, and I updated `plan.md` to say so instead of leaving a
resolved risk standing. No rework followed — the default I had committed
to in my claim is the direction he gave — which is the argument for
asking the question at claim time rather than at review time.

**Toolkit**

- Tool: `CLAUDE.local.md`, the repo-memory file from unit 6, carrying the
  conventions and the subsystem map into every session in this clone.
- What it produced: the conventions block (conventional commits, the PR
  template's required fields, `pytest` and `flake8`, no stated AI
  policy) and a map of where each FAQ answer's behaviour lives, so I did
  not re-read `hardware.py` every session.
- Difficulties: its gap was silence, not error. It says nothing about
  the package's import chain, so when I concluded from reading
  `core.py` that `import ecotrace` leaves `matplotlib` alone, the memory
  file had nothing to contradict me with. A memory file only protects
  you from re-deriving what it already holds; it cannot warn you about
  what nobody thought to write down.
- What changed: I added the verified import chain to it — `ml.py:10`
  imports the reporting module at module level, `__init__.py` imports
  `ml`, so matplotlib and fpdf load on every import, ~0.46 s of ~0.53 s
  — along with the note that `_check_rapl` catches `PermissionError` and
  not just a missing path, which is what later stopped me promising the
  bind mount. Both entries are written from command output rather than
  from reading.

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
