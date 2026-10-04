# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/88

**Branch**

fix/53-phone-space-separator

**My pr-precheck verdict on this draft**

I ran my own tool in live mode on the draft before opening the PR, from the working copy
on the branch:

```
claude "pr-precheck: grade my draft PR for issue https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53: plan in plan.md, the diff on this branch, title and description in pr_draft.md, test evidence in test_evidence.md"
```

Verdict: **`accept`**, with all five required checks and both preferred checks passing.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53 (branch fix/53-phone-space-separator)",
  "checks": [
    {"name": "diff-within-plan", "grade": "pass", "evidence": "Diff is the phone_us regex line plus removal of the four issue-named xfail markers; the \\b -> (?<!\\w) change is covered by the plan's Deviations note; test_mixed_pii_and_text marker kept as planned."},
    {"name": "description-matches-diff", "grade": "pass", "evidence": "Changes bullets name exactly the [-.\\s]? widen, the (?<!\\w) lookbehind, and the four marker removals; the kept fifth marker and the wider match span are disclosed in Notes for Reviewers."},
    {"name": "evidence-decisive", "grade": "pass", "evidence": "Issue snippet before 'Call me at (555) 123-4567 or [REDACTED]' / detect [] vs after 'Call me at [REDACTED] or [REDACTED]' with phone_us detection; -k phone 6 passed incl. controls; unit suite 375/53 -> 379/49 (independently reproduced)."},
    {"name": "diff-reviewable", "grade": "pass", "evidence": "2 files, +1/-17: one regex line and four deleted xfail decorators; no debug output, dead code, or formatting churn."},
    {"name": "standards-and-disclosure", "grade": "pass", "evidence": "All PR template sections filled with real content, CI box honestly unticked with reason, 'Closes #53', Conventional Commit 'fix(safety): ...' with 'Fixes #53', and an AI-use disclosure present."},
    {"name": "commits-scoped", "grade": "pass", "evidence": "Single commit 2dcadad 'fix(safety): redact parenthesized and spaced US phone numbers' whose body describes exactly the diff; no wip/fixup debris."},
    {"name": "reviewer-ready-description", "grade": "pass", "evidence": "Summary states the cause and approach in one paragraph; 'Closes #53' in the Issue section."}
  ],
  "verdict": "accept"
}
```

The run did more than return a verdict: it independently rebuilt the environment and
re-ran my claims against a temporary worktree of `main` rather than trusting
`test_evidence.md`, and it caught a real accuracy problem I fixed before opening the PR.
I had run the repo's checks against only the changed files while labelling them with the
`make` targets, which run repo-wide (`make typecheck` covers 76 source files, not 1;
`make format` covers 110 files, not 2). I re-ran all five the way the Makefile defines
them and replaced the output, which also surfaced that `make test-integration` exits
non-zero (`Error 5`) because `tests/integration/` holds only `__init__.py` and pytest
collects nothing. That is now disclosed in the PR's Testing section rather than quietly
ticked.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Before spending any credit I hand-graded the four calibration packages against my draft
tool. That produced no agreement score, but it caught a real defect and I revised before
running the harness.

The trap, `calib-03`, my `diff-within-plan` check caught cleanly: its description claims
"no changes beyond the plan" while the diff's second hunk silently rewrites
`src/language-js/print/property.js`, a file the plan never names. But `calib-04` exposed a
hole. Its plan's test plan names two failure modes by name (the `-j` panic and the
`-j 200000 -x echo` abort), and its evidence re-runs only the first. My
`evidence-decisive` check as originally written would have passed it, because it did show
a before/after and did run `cargo test`. I rewrote the check to key on covering the
plan's test plan rather than merely showing a trigger, which is what the gold label
rejects on.

Scored runs, in order:

1. **20/20 scored items, bar: PASS.** Categories: clear-accept 7/7, not-tested 4/4,
   silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. Every category matched,
   including the 2-package `standards-wall` category the eval README flags as the live
   case this week.

This is the run recorded in `eval-run.txt`, and 20/20 is its agreement line. There was one
scored run because the first one agreed with every gold label; there was nothing to
revise.

**Package analysis**

**`pkg-13`** (neovim). **My tool: `accept`. Gold label: `accept`. Agreed.**

I picked this one because it is the package that tests whether a rubric understands the
honest-outcome rule rather than just punishing incompleteness. `pkg-13` implements only
part of what a reader might expect: it caret-escapes the cmd.exe metacharacters
(`& ^ | < >`) so `vim.ui.open("...?q=hello&t=brave")` stops truncating, but it explicitly
does **not** handle percent-sign environment expansion, so a URL like `?q=%PATH%` still
expands. A rubric that equated "less than everything" with "hold" would reject it.

All seven of my checks passed it. The deciding one is `diff-within-plan`: the shortfall is
not silent. The plan recorded the `%VAR%` deferral before the PR, and the PR's description
restates it ("Deviation note, recorded in the plan before this PR: percent-sign
environment expansion (`%VAR%`) is NOT handled here"). My check's pass condition treats a
recorded deviation as re-tying the mismatch, so a disclosed deferral passes and only a
silent one fails. `evidence-decisive` passed on the before/after against the exact repro
command (`code = 1` with a truncated URL before, `code = 0` with the full URL after) plus
`make functionaltest` shown green, and `standards-and-disclosure` passed on stated
silence: neovim's repo facts state no AI-disclosure requirement, so that axis has nothing
to honor.

The contrast with `pkg-17` in the same run is what convinced me the rule is drawn in the
right place: `pkg-17`'s plan promises a dev warning *and* a docs update, the diff delivers
only the warning, and nothing records the gap. Same shape of shortfall, opposite verdict,
and the only difference is whether it was disclosed.

**Check rationale**

The check I revised during calibration, quoted as it currently reads in
`tools/pr-precheck/rubric.md`:

> | evidence-decisive | The test-evidence section, read against the plan's test plan
> (every trigger and failure mode it names) and the reproduction's own steps, plus the
> outcome of the repo's own checks or suite. | Passes when the evidence re-runs the
> trigger(s) the plan's test plan names, exercising the path the fix changed, and shows an
> observable before and after for each (the symptom present before, the expected-after
> present after), AND the repo's own tests or checks are shown run with their outcome
> visible. Fails when the evidence proves nothing observable ("tested locally", "verified
> working", "tests pass" with no before/after), exercises a path the fix did not change,
> omits a failure mode the plan's test plan explicitly named, or never runs the repo's
> checks. | required |

Three clauses, each answering a package that would otherwise slip through. The
"observable before and after" clause catches the `not-tested` packages whose evidence is a
sentence: `pkg-04`'s "tested locally" and `pkg-10`'s "verified working" prove nothing a
reader can check. The "exercising the path the fix changed" clause catches `pkg-14`, whose
evidence runs a GET with header casing while the fix was on a different path, so the test
would have passed before the change too.

The third clause, "omits a failure mode the plan's test plan explicitly named", is the one
calibration forced. My first draft asked only for *a* re-run with a before and after, and
under that wording `calib-04` passes: it re-runs repro 1, shows a clean argument error
instead of the panic, and runs `cargo test`. But its plan named *both* failure modes in
the test plan, and the evidence is silent on the `-x echo` abort, which is a different
code path through the same fix. Measuring the evidence against the plan's own test plan,
rather than against the existence of some before/after, is what separates those two cases.

**Trade-offs**

What `evidence-decisive` gives up is that it measures coverage against *what the plan said
it would test*, not against what the change actually risks. A plan with a thin test plan
drags the bar down with it: if a plan names one trigger and the evidence re-runs that one
trigger with a clean before and after, this check passes, even when the diff plainly
touches a second code path the plan never thought to mention. The check cannot reach a
failure mode nobody wrote down.

I accepted that deliberately, because the alternative is worse. To catch an untested path
the plan never named, the check would have to infer from the diff which behaviors *ought*
to have been tested, which is the tool inventing a test plan at grading time and rejecting
contributors on its own guesswork. Every other check in this rubric grades a stated
artifact against another stated artifact; this one would become the exception that grades
against an imagined one. The honest limit is that a careless plan can buy a careless PR a
pass here, and the place to fix that is week 3's plan rubric, where the test plan is
written, not week 4's.

Nothing else in the run moved as a result, and I can say that precisely: this revision
happened before the only scored run, which agreed on all twenty packages, so there is no
earlier scored run for it to have changed. The change was validated against the
calibration set rather than against gold-labelled scored packages: `calib-04` flipped from
pass to fail (matching its gold `reject`), while `calib-01` stayed accept, confirming the
tightening did not start rejecting honest evidence.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
