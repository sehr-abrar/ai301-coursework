# Procedure: how this tool grades a PR package

This is the operating procedure the tool executes. Follow it exactly, in
order. Where a step says to record something, hold that note for the
check that uses it. The graded object is a PR package, and the reads that
decide it are side-by-side: the diff against the plan, the evidence
against the test plan, the description against the diff. Gather both sides
of each pairing before grading the check that compares them.

## Read order

Read the package in this order, before grading any check. The plan comes
before the diff on purpose: the diff can only be judged against the
promise it claims to deliver, so the promise must be in hand first.

1. **Issue context first.** Read the issue title and body. Record the
   symptom in one line: the specific wrong behavior the PR must fix.
2. **Plan context second.** Read the plan the PR claims to implement.
   Record its scope pair (the in-scope change list and the not-in-scope
   line), its test plan (the trigger and the expected-after), the repro
   evidence it built on, and any deviation notes. These notes are ground
   truth for `diff-within-plan` and `description-matches-diff`, so capture
   them before reading the diff.
3. **Repo facts third.** Record the repo's stated PR-template sections or
   required checklist, its contribution policy, and specifically whether
   it requires AI-use disclosure. This note decides
   `standards-and-disclosure`.
4. **The diff fourth.** Now read the unified diff and the commit list.
   List every changed file and, for each, whether it falls inside the
   plan's scope, is covered by a deviation note, or is unaccounted for.
   Note any debris (debug leftovers, commented-out code, formatting churn,
   unrelated hunks) while reading.
5. **The description fifth.** Read the PR title and description. List the
   changes it claims and the limitations it discloses, to compare against
   the diff you just read.
6. **Test evidence last.** Read the test-evidence section against the
   plan's test plan from step 2: is the reproduction's trigger re-run with
   an observable before and after, and are the repo's own checks shown run?

## Evidence gathering

For each check, pull its evidence from the notes taken during the read,
using `references/evidence-guide.md` for where each family lives. Each
load-bearing gather is a pairing; assemble both sides before grading.

- `diff-within-plan`: the changed-file list from read step 4, paired with
  the scope pair and deviation notes from read step 2.
- `description-matches-diff`: the description's claims from read step 5,
  paired with the actual diff contents from read step 4.
- `evidence-decisive`: the test-evidence section from read step 6, paired
  with the plan's test plan and repro trigger from read step 2.
- `diff-reviewable`: the debris notes from read step 4 (diff and commits).
- `standards-and-disclosure`: the description and commits from read step
  5, paired with the repo's stated asks from read step 3.
- Preferred checks (`commits-scoped`, `reviewer-ready-description`):
  gather only after every required check has passed; their grades never
  change the verdict, so on an already-rejected package gather them only
  to report.

In live mode, gather the plan from `plan.md`, the diff from
`git diff main...HEAD` on the working copy, the title and description from
the draft file, the test evidence from the captured output file, and the
issue, template, and policy from the scoped repo per the evidence guide.
In eval mode, every fact is quoted from the bundle text.

## Check execution

Execute the required checks in this fixed order, so two executors grade
the same package the same way: `diff-within-plan`,
`description-matches-diff`, `evidence-decisive`, `diff-reviewable`,
`standards-and-disclosure`.

For each check:

1. Apply the rubric's pass condition to the gathered evidence for that
   check.
2. Grade `pass`, `fail`, or `unclear`, and record one line of evidence:
   the exact fact, quote, diff line, or missing item that decided it.
   Never grade without naming the fact.
3. When the evidence a check needs is genuinely absent from the package,
   grade `unclear` (not `fail` by assumption, not `pass` by benefit of the
   doubt) and say what was missing. The verdict rule converts `unclear` to
   a hold, but the distinction is preserved in the output.
4. The one silence-passes case is written into `standards-and-disclosure`:
   where the repo states no requirement on an axis (no template, no
   disclosure rule), that axis is a genuine pass, not `unclear`.
5. Treat a recorded deviation note as re-tying the mismatch it covers:
   a disclosed shortfall or scope change passes `diff-within-plan` and is
   accounted for by `description-matches-diff`; only a silent one fails.

A check is graded from its gathered evidence; do not re-read the whole
package per check once the read order is done. Re-read one section only if
a check's evidence note is ambiguous.

## Verdict assembly

1. Collect the five required grades.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail`, or any required check is `unclear`
   (which counts as fail per the rubric), the verdict is `reject`.
4. Preferred checks never enter this computation. Report their grades; on
   an accepted PR, cite them as reasons it is easy to review.
5. In the output, quote the deciding check: for a `reject`, the first
   required check that failed in the fixed execution order above; for an
   `accept`, the check whose evidence most directly shows the PR delivers
   its plan cleanly. Emit the JSON block last, exactly as SKILL.md
   specifies, one entry per check in execution order followed by the two
   preferred checks.
