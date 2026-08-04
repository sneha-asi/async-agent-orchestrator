# The Loop (replaces the intake-layer idea)

How a task actually gets done. This replaces an earlier design that tried to perfect the specification before building. That approach was waterfall in a voice interface, and it is rejected. See the bottom of this doc for why.

---

## 1. The first principle

You cannot reliably know what you want before you see something. "No, not like that" is the most information-dense sentence in software, and it requires an artifact to point at. Specification is not a transfer problem (clarity in your head, moved to the agent). It is a discovery problem (clarity created by reacting to something concrete).

Therefore:

> **Do not minimize the probability of being wrong. Minimize the cost of being wrong.**

Every design decision below falls out of that one line.

## 2. The cost of being wrong, broken into parts

If a wrong build costs you `C`, then:

```
C  =  time to first artifact
   +  time to notice it is wrong
   +  cost to correct it
   +  risk of damage it caused
```

You attack each term. That is the entire product.

| Term | How it is attacked |
|---|---|
| **Time to first artifact** | Build the smallest useful slice first, not the whole task. Use a fast/cheap model tier for the first pass. No long planning phase before anything exists. |
| **Time to notice** | Show something concrete early and continuously, not a completion report at the end. The narration layer exists for this. Wrong work should surface in minutes, not at the end of the task. |
| **Cost to correct** | Correction is a reply, not a new task. Context is preserved, so "no, the other dashboard" costs one sentence. Never make the user re-explain from scratch. |
| **Risk of damage** | Branch-only work, mechanical guardrails, nothing lands on main without gates. Throwing work away must be free. (Already covered in the Design Constitution.) |

## 3. What this changes about the design

**Smallest useful slice, not the whole task.** The default unit of work is the smallest thing that produces something you can react to. A big task becomes a sequence of these, each one confirmed cheaply, rather than one long build confirmed at the end.

**Show, do not report.** "Added a login screen, tested it, works" is a claim. A screenshot of the login screen is an artifact. Wherever the project type allows an artifact, produce one:

| Project type | What "show something concrete" means |
|---|---|
| Frontend / web app | a rendered screenshot or a live preview link |
| Backend / API | the actual request and response, real output not described output |
| Data / pipeline | a sample of the transformed rows, before and after |
| Script / CLI | the command run and its real terminal output |
| Anything with tests | the test run output, pass or fail, unedited |

This is also the honest fix for the narration-can-lie risk. An artifact cannot misreport itself the way a summary can.

**Correction is cheap and expected, not a failure.** The system should treat "that is wrong, do it this way" as the normal path, not an exception. Discarding a branch costs nothing and should never be framed to the user as wasted work. Being wrong fast is the mechanism, not a bug in it.

**Attention is the scarce resource, so spend it on reacting, not on interviewing.** Reacting to something concrete is far lower cognitive cost than answering abstract questions about a thing that does not exist yet. This is why the loop fits the north star ("work without working") and an upfront interview does not: an interview demands sustained focus at exactly the moment you wanted to hand off and walk away.

## 4. What survives from the old intake idea

Small, but real:

- **Clarifying questions, strictly bounded.** Ask only when a wrong guess would be expensive AND cannot be discovered cheaply by just building the slice. Default is to make a reasonable assumption, state it plainly, and build. One question beats zero only when building blind would waste real time.
- **Stated assumptions.** Every task starts by saying what it assumed, in one line. This is cheap, needs no reply, and lets you catch a bad assumption without an interview.
- **Images in, always.** Showing beats describing, especially for anything visual ("it looks wrong", layout, a whiteboard sketch, a competitor's screen). This stands on its own merit and does not depend on any of the rest.
- **A short brief, demoted.** Not a gate the user must approve before work starts. It exists because the PR-reviewer model needs something to check the finished code against ("did it build what was asked?"). It is a downstream artifact, not a front door.

## 5. What is rejected, and why

- **Upfront interview before building.** Waterfall. Assumes clarity exists to be extracted, demands attention at the worst moment, and delays the artifact that would have produced real clarity anyway.
- **Brief-as-approval-gate.** Blocks work on a reply, which breaks async. Keep the brief, drop the gate.
- **Voice as a differentiator.** Voice is a convenience for input speed, nothing more. The design does not depend on it, and every competitor has voice already. It is not a wedge.
- **Video calling.** The agent gains nothing from seeing a face. Real infrastructure cost, zero capability gain.

## 6. The one-line version

Show me something small and real fast, and make being wrong cost one sentence to fix.
