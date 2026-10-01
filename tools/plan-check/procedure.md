# Procedure: how this skill grades a plan package

This is the operating procedure the skill executes. Follow it exactly,
in order. Where a step says to record something, hold that note for the
check that uses it. The rubric (`rubric.md`) says what each check
decides; this file says how to reach each check, in what order, and how
the grades become a verdict.

## Read order

Read the package in this order, before grading any check. The order
matters because every check measures the plan against evidence that must
already be in hand when the check runs.

1. **Issue context first.** Read the issue title and body. Record the
   symptom in one line: the specific wrong behavior being reported.
2. **Repro-evidence block second, and most carefully.** This is ground
   truth. Record: the failing trigger (the exact command or input), the
   observed failure (exit code, output, error), and every control run
   with what it establishes (what succeeds, and therefore what layer is
   isolated). These control-run notes decide `diagnosis-grounded`, so
   capture them before reading the plan and forming an opinion.
3. **Repo-facts block third.** Record the stated contribution policy,
   specifically whether it requires AI-use disclosure, and any stated
   bug-report or contribution asks.
4. **Thread highlights fourth.** Record any explicit maintainer
   direction: an owner-isolated culprit, a proposed approach, a posted
   patch, a request for prior art. If there is none, record "no
   maintainer direction". This note decides `comment-engages-thread`.
5. **Candidate plan fifth.** Now read the plan: its diagnosis, change
   list, not-in-scope line, files/areas, approach, test plan, risks and
   unknowns (and any deviations note). Read it against the notes above,
   not on its own terms.
6. **Candidate plan comment last.** Read it against the thread note and
   the policy note from steps 3 and 4.

## Evidence gathering

For each check, pull its evidence from the notes taken during the read,
using `references/evidence-guide.md` for where each family lives. Gather
before grading; do not grade a check while still hunting for its fact.

- `diagnosis-grounded`: the plan's cause sentence, plus the control-run
  notes from read step 2.
- `scope-bounded`: the plan's change list and not-in-scope line, against
  the one-line symptom from read step 1.
- `plan-executable`: the plan's files/areas line and approach statement.
- `test-plan-observable`: the plan's test-plan line, against the failing
  trigger recorded in read step 2.
- `honesty-calibrated`: the plan's risks/unknowns and deviations note,
  against what the repro evidence actually establishes.
- `comment-engages-thread`: the plan comment, against the maintainer-
  direction note from read step 4.
- `repo-conventions-met`: the plan comment, against the policy note from
  read step 3.
- Preferred checks: gather only if every required check has passed;
  their grades never change the verdict, so they are not worth gathering
  on an already-rejected package except to report.

In live mode, gather issue-side evidence (issue, thread, policy) from
the locations the evidence guide names, and take the reproduction
evidence from the student's posted repro comment; the drafts are the
candidate plan and comment. On the house issue with no posted repro of
the student's own, the reproduction evidence is only what the drafts
quote, and a check whose evidence the drafts never quote is graded on
that absence.

## Check execution

Execute the required checks in this fixed order, so two executors grade
the same package the same way: `diagnosis-grounded`, `scope-bounded`,
`plan-executable`, `test-plan-observable`, `honesty-calibrated`,
`comment-engages-thread`, `repo-conventions-met`.

For each check:

1. Apply the rubric's pass condition to the gathered evidence.
2. Grade `pass`, `fail`, or `unclear`, and record one line of evidence:
   the exact fact, quote, or control-run contradiction that decided it.
   Never grade without naming the fact.
3. When the evidence a check needs is genuinely absent from the package,
   grade `unclear` (not `fail` by assumption, and not `pass` by
   benefit of the doubt) and say what was missing. The verdict rule
   converts `unclear` to a hold; the distinction is preserved in the
   output so the student sees whether a check failed or could not run.
4. The two silence-passes conditions are genuine passes, not `unclear`:
   `comment-engages-thread` when the thread note is "no maintainer
   direction", and `repo-conventions-met` when the policy note states no
   requirement.

A check is graded from its gathered evidence; do not re-read the whole
package per check once the read order above is done. Re-read one section
only if a check's evidence note is ambiguous.

## Verdict assembly

1. Collect the seven required grades.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail`, or any required check is `unclear`
   (which counts as fail per the rubric), the verdict is `reject`.
4. Preferred checks never enter this computation. Report their grades;
   on an accepted plan, cite them as reasons to prefer it.
5. In the output, quote the deciding evidence: for a `reject`, the line
   from the first required check that failed (in the fixed execution
   order above); for an `accept`, the check whose evidence most directly
   shows the plan is grounded and buildable. Emit the JSON block last,
   exactly as SKILL.md specifies, with one entry per check in execution
   order followed by the two preferred checks.
