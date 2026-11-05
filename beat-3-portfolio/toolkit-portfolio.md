# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Sehr Abrar

## Featured Tools

I'm featuring four: issue-scout, pr-precheck paired with the ship-gate, the voice-guard, and the pr-monitor. These are the four where I can point at a specific date, a specific wrong thing I wrote, and a specific line in a log file that caught it. repro-check and plan-check did real work too — they're in the Inventory below — but their catches were one-off rather than a pattern I can trace across the term, so I left them out of the depth slots rather than pad the count to four-and-two-halves.

### issue-scout

**What it is, and the judgment hole I wrote:** A search-and-grade tool over `gh search issues`: it runs my own queries, ranks results with my own filters, and grades the survivors against a rubric I wrote and keep patching. The judgment is in `scope.md` (my fit profile: "I am most comfortable reading and debugging in Python and JavaScript. Search Python and JavaScript first... So look for newcomer-signposted work: `good first issue`, `help wanted`, and `documentation` labels on open, unassigned issues") and in `rubric.md`, where I wrote the `maintainer-responsive` check myself and later rewrote it.

**How it evolved, dated:**
- **2026-10-21, scan 1.** My first live scan returned 379 issues, and when I checked the pool directly I found every single one had been created within the previous 24 hours. `gh search issues` returns newest-first with no date floor of its own, so my "drop anything not updated in 90 days" filter had dropped exactly 0 of 379 — it was structurally incapable of ever firing.
- **2026-10-21, same day, second attempt.** I added `--created`/`--updated` date windows to the queries. That didn't work either: the windows just slid where the newest-100 ceiling landed, so every result still came back with zero age spread. My replacement ranking filter was worse — it dropped 100 of 100 real results, because most issues are never touched again after being filed.
- **2026-10-21, third attempt, same day.** I added a search *term* to every query instead of a date window. That's what actually worked: coverage went from effectively one repo at zero-day median age to 63–85 distinct repos with back-catalogue depth over 1,900 days.
- **2026-10-21, later.** Patched `maintainer-responsive` in the rubric to count a merged outside pull request as a reply, not just an issue comment — written as an additional way to pass, never a new way to fail, so my unit-1 eval bundles still grade the same.

**Evidence:** The rubric line itself, verbatim: *"A merged pull request counts as a reply... Zwony/ecotrace has zero owner comments across 40 issues and was graded `fail`, while merging outside contributors' PRs in 23 minutes, 4.5 hours and 14 hours."* I verified that patch on a real issue the same day, and the thing it got right for the wrong reason showed up a week later: the maintainer who "never replies to issues" replied to mine in 31 minutes, because I was the first person in 40 issues to actually ask him something. My rubric had measured "does anyone reply here" when the real signal was "has anyone given them something to reply to" — a gap I still can't grade from issue metadata.

### pr-precheck, paired with the ship-gate

**What it is, and the judgment hole I wrote:** pr-precheck is a 7-check rubric for a pull request (diff-within-plan, description-matches-diff, evidence-decisive, diff-reviewable, standards-and-disclosure, plus two preferred checks); the ship-gate is the hook that runs it against my real branch before `gh pr create` is allowed to go out. My judgment lives in `ship-manifest.md`, sitting at the root of my working clone rather than in the skill folder — it names the two checks I decided actually mattered for my change (`pytest`, and `mkdocs build`, in my own words: *"Exits 0 on `main`, and warns when a `nav:` entry points at a file that does not exist — which is exactly the mistake this PR could make"*) and excludes two more with dated reasons (`flake8`, 759 pre-existing findings; `mkdocs build --strict`, 60 pre-existing warnings, both on `main` before I touched anything).

**How it evolved, dated:** It didn't need to evolve — it caught me working as designed, four times in a row on my branch before the PR ever existed on GitHub, then once more after a real reviewer weighed in. Rejection 1: my diff stat claimed 189 insertions when the real diff was 188, and my description said the FAQ "links out for the error figures" when the page actually quoted them. Rejection 2: a line in my FAQ contradicted a commitment my own plan had made (treating an unsourced "≤15%" claim as authoritative after I'd promised to label it as unsourced), and my test evidence had been generated against a commit that was no longer the one under review. Rejection 3: a grep table in my description read `RAPL → 15` after a later rewording had changed the real count to 13. On the fourth run it passed everything.

**Evidence:** From `gate-log.md`, the final run, fingerprinted to commit `34da8c1`: *"verdict: PASS... diff-within-plan [pass]: Diff touches only docs/FAQ.md and mkdocs.yml... description-matches-diff [pass]: Every named change... is in the diff; the untested Docker caveat is disclosed... evidence-decisive [pass]: FAQ.md absent and no nav entry on main vs... site/FAQ/index.html on branch... commits-scoped [pass]."* Every one of the four rejections before that was a real, checkable defect, not a style nit — each one was a number that had drifted after I edited prose, which is exactly the failure mode a human reviewer would never have caught either, since none of it would have read as a lie on the page.

### voice-guard

**What it is, and the judgment hole I wrote:** A hook that reads every outgoing `gh` comment against my own voice guide before it posts. The judgment is the guide itself — five rules under an "Upstream" section I wrote in unit 6, each with a Wrong/Right pair, covering how I claim issues, ask questions, and send nudges.

**How it evolved, dated:** On **2026-11-01** my own rule contradicted itself across two of my tools in the same afternoon. The voice-guide rule I'd just written said a claim should promise a report-back date. repro-check — a different tool, grading the same draft — rejected it for exactly that, because its own rubric fails any timing commitment, soft or not. repro-check was right: at claim time I hadn't read the code yet, so a report-back date was as invented as a delivery date. I rewrote the rule the same day to promise only a report-back with no date attached, and kept the original wording inside the rule as a second "Wrong" example, on the theory that a guide telling me to write what my own checker rejects is worse than no guide.

**Evidence:** `guard-log.md`'s very first line is the wiring test doing its job: *"2026-11-01 14:44 [hook] stop: no body the guard can read (interactive and --help forms are stopped on purpose) | target: gh issue comment --help"* — proof the hook fires before anything goes out. The claim-draft revision cycle shows up as four more `[manual] pass` lines over the next 40 minutes that same day, each one a round of me fixing the draft and re-checking it until the rewritten rule and the draft agreed.

### pr-monitor

**What it is, and the judgment hole I wrote:** A tool that reads a live PR's state and drafts the next move — a reply, a wait note, a nudge — against a playbook I wrote. The judgment is `playbook.md`'s Staleness thresholds section, built from measuring the repo's actual merge rhythm rather than guessing: *"the gap from opened to merged was a median of 5.8 hours and a maximum of 13.7 hours... First response: 3 days of silence... this is the rule I expect to be most tempted to break, because my submission is due Sunday and his schedule is not a fact about my course."*

**How it evolved, dated:** It caught its own bug the first time I ran it. On **2026-11-02 at 23:07**, the free `--state` canary (no model call) labelled my PR `FEEDBACK-IN` because the only thread entry was a SonarCloud bot comment — the canary has no way to tell a bot comment from a human review. The full run, seconds later, caught what its own canary had missed: it drafted "Wait note. Nothing to send," naming the misread outright ("writing one would mean inventing a reviewer's comment") and citing my 3-day threshold against a PR that was six minutes old. Two days later, on **2026-11-04**, it got a real review to work with — the maintainer's actual changes-requested review with four inline comments — and its draft matched only three of the four, because it matched by commit-message theme rather than by the actual diff hunks he'd commented on. I rewrote the reply myself, checking all four against the source again before sending it.

**Evidence:** `monitor-log.md`, two entries two days apart, same PR: `2026-11-02 23:07 [state]` reads *"feedback-in: review feedback is in (latest from the thread: comment under a day ago)"* while the paired `[run]` the same minute reads *"move: Wait note. Nothing to send. (via the review response form)"* — the canary and the full run disagreeing on the same state, with the full run winning. The `2026-11-04 19:11 [run]` entry against the real review is on the public record too: the review it answers is visible on `https://github.com/Zwony/ecotrace/pull/248`, with four dated inline comments from the maintainer and my reply underneath them.

## Inventory

- **repro-check**: graded my reproduction report on issue #215 and rejected two overstated claims before passing it — I'd written that a troubleshooting entry was "in the hardware support tables" and that a string appeared "nowhere in the repository" when my grep had only searched two file types. Both fixed by pasting the actual command output next to the claim instead of describing it from memory. Stable since; no further patches needed.
- **plan-check**: rejected my first fix plan for quoting an unsourced "typical" accuracy figure as if it were a measured result, and for treating an untested Docker fix as settled when I had no way to run it on my hardware. I also found a gap in its own `procedure.md` — it assumed a posted repro comment always exists, which isn't true for a docs issue — and patched that file so it states how to grade evidence quoted straight from the repository when there's nothing to post.

## The Arc

**Beat 1**
Date: 2026-10-21
Friction: issue-scout's first live scan returned 379 issues, every one of them filed within the previous day, because `gh search issues` returns newest-first and I'd given it no way to reach further back; a staleness filter I was relying on had dropped exactly 0 of 379 results and could not possibly have fired.
Change: Rewrote the search strategy twice in one sitting — first with date windows (which just slid where the ceiling landed), then with a search term on every query, which is what actually reached the back catalogue.
Result: Coverage went from one repo at zero-day median age to 63–85 distinct repos with issues up to 1,900+ days old; the same day, I verified a separate rubric patch (`maintainer-responsive`, counting a merged PR as a reply) against a real repo and confirmed it flipped a false rejection to an accept.

**Beat 2**
Date: 2026-11-01
Friction: My own voice-guide rule and repro-check's rubric disagreed about my claim draft on the same afternoon — the guide said to promise a report-back date, repro-check's rubric rejected any timing commitment at all, soft or not.
Change: Rewrote the voice-guide rule to promise only a report-back with no date, keeping the original wording inside the rule as a labelled Wrong example.
Result: The revised claim passed both tools and posted to the issue thread the same day; the maintainer replied in 31 minutes, the first reply he'd given across at least 40 prior issues.

**Beat 3**
Date: 2026-11-02 to 2026-11-04
Friction: The ship-gate rejected my branch four separate times before I ever opened a pull request — a stale diff-stat number, FAQ wording that contradicted my own plan's commitment, test evidence generated against a superseded commit, and a grep count a later edit had quietly changed.
Change: Stopped hand-copying numbers into the draft description and started regenerating every one directly from the committed file before each gate run.
Result: Fourth run passed all seven pr-precheck checks, logged in `gate-log.md` with verdict PASS and fingerprinted to commit `34da8c1`; I pushed and opened the pull request against the real repo on that commit the same day.

**Beat 4**
Date: 2026-11-02 to 2026-11-04
Friction: The pr-monitor's free state canary mislabelled a SonarCloud bot comment as human review feedback, and two days later its paid run under-matched a real four-comment review to only three fixes by matching commit messages instead of diff hunks.
Change: Trusted the full (paid) run over the free canary when they disagreed, and rewrote the monitor's draft reply by hand, re-checking all four of the maintainer's comments against the source myself before posting.
Result: The first run correctly sent nothing, against a PR that was six minutes old; the second produced a reply that, once I'd fixed it, the maintainer is now reviewing — all four of his comments addressed in one commit each, visible on the PR thread.

## Limitations and What's Next

Every tool in this toolkit grades or drafts text — none of them verify that the text is *true* about the code. That job stayed mine the whole term: repro-check, plan-check, and the ship-gate all caught me asserting something I hadn't actually checked, and every one of those catches was still mine to resolve by going back to the interpreter or the source file, not the tool's to resolve for me. The toolkit also has no cross-tool awareness — the voice-guide rule that repro-check rejected was a rule I'd write into a different file, in a different tool, with no mechanism linking the two; I only noticed the contradiction because I happened to run both the same afternoon. And every tool here reads GitHub metadata, never code semantics: issue-scout's biggest miss (reading "no replies" as "unresponsive maintainer" instead of "nobody asked a question") and the monitor's biggest miss (matching review comments to commits by theme instead of by diff hunk) are the same category of gap — text-level matching standing in for actually understanding what a comment or a commit is about.

What I'd build next: a step between the ship-gate and the PR-monitor that diffs a maintainer's review comments against the actual hunks in the diff, rather than against commit messages, so the monitor's draft stops under-matching real reviews the way it did on 2026-11-04. Second, I'd add a lightweight cross-tool check — something that reads all of my rubrics and my voice guide together once a term and flags direct contradictions, instead of relying on me noticing by accident the way I did in Beat 2.

**Tool limits.**

issue-scout: can tell whether a thread has activity but not whether anyone has actually asked the maintainer a question, so it reads "no replies" as unresponsive when it can really mean "nobody asked."
pr-precheck + ship-gate: needs a human to keep every number in the description synced to the diff by hand before each run; it catches drift after the fact but does nothing to stop me from causing it.
voice-guard: has no visibility into any other tool's rules, so two of my own files can tell me to do contradictory things and the guard will faithfully enforce whichever one it's pointed at.
pr-monitor: matches a maintainer's review comments to my fixing commits by theme and commit message rather than by the actual diff hunk commented on, which under-counted a real four-comment review to three.
