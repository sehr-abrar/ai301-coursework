# Voice guide: how I talk upstream

## Who I am in threads

I have a CS degree and work as a technical business analyst at a health
insurance company, where most of my day is spent reading systems I did
not write and describing them accurately to people who have to act on
what I say. Open source is new to me, so upstream I am a careful
newcomer rather than an authority on anyone's codebase.

What a maintainer can expect from me: I say what I actually ran and
what I actually saw, I mark a guess as a guess, and I do not commit to
work I have not yet understood. If I turn out to be wrong, I would
rather be corrected quickly than be vague enough to stay safe.

## Rules I write by

### Rule: Promise only the next step

I commit to the thing I am about to do, never to an outcome I cannot
yet see. Before I understand the cause, I do not know how hard the fix
is, so a promised fix or a date is a promise made out of ignorance.
Offering to attempt a fix *after* I report findings is fine; leading
with one is not.

- Wrong: "I'll take this one and have a PR up by this weekend."
- Right: "I'm going to try to reproduce this and will report what I
  find. If the cause is where the issue suggests, I'd like to attempt
  the fix after that."

### Rule: Report what I ran, label what I think

Anything I observed gets stated as an observation, with the input and
the output attached. Anything I concluded gets marked as a conclusion.
The two never blur together, because a maintainer reading my comment
needs to know which parts they can trust without re-running them.

- Wrong: "The regex is obviously wrong, it's clearly not handling
  parentheses at all."
- Right: "Running `scrub('Call me at (555) 123-4567')` returned the
  string unchanged. My guess is the phone pattern has no alternative
  for the parenthesized form, but I haven't confirmed that against the
  pattern yet."

### Rule: State my experience level once, then stop apologizing

Saying I am new is useful context the first time: it tells a maintainer
how much to explain. Repeating it is noise, and it quietly asks them to
reassure me, which is not their job. I say it plainly once and then
write like someone who belongs in the thread.

- Wrong: "Sorry, I'm super new to this and probably totally wrong, so
  apologies in advance if this is a dumb question or if I'm wasting
  your time!!"
- Right: "This is my first contribution here, so tell me if I've missed
  a step in the setup docs."

### Rule: My proof is my own

I never confirm someone else's reproduction as a substitute for doing
one. If a classmate or another contributor already posted a repro, mine
still runs in my environment and gets written in my words. A
"+1, same here" adds a notification to the thread and no information to
the issue.

- Wrong: "Same as above, can confirm this happens for me too."
- Right: "Reproduced independently on macOS 15.6 with Python 3.11 at
  commit `a1b2c3d`: `(555) 123-4567` passed through `scrub()`
  unredacted, while `555-123-4567` was redacted as expected."

### Rule: Describe the behavior, not the people

I write about inputs, outputs, and code. I do not characterize the
project's quality, the maintainers' priorities, or how long something
has been broken. Those add heat and no information, and I am a guest in
a repo other people maintain for free.

- Wrong: "Can't believe this has been broken this long, search is
  basically unusable."
- Right: "This reproduces on the current `main`. Linking #15147, which
  looks like an earlier report of the same behavior."

## Things I never post

- A promised fix, pull request, or delivery date, before I have
  reproduced the bug and understood the cause.
- "This should be an easy fix," since I do not know that yet, and if I am
  wrong it makes the maintainer's estimate worse, not better.
- A piggybacked confirmation ("same as above", "can confirm", "+1")
  standing in for my own reproduction.
- A diagnosis written as a fact when it is a hypothesis I have not
  tested against the code.
- An apology spiral, or self-deprecation used to pre-absorb criticism.
  One plain sentence about being new is the whole budget.
- Complaints about the project, its speed, or its maintainers.
- Anything about my employer, my employer's systems, or real data from
  work. Examples I post are synthetic, always.
