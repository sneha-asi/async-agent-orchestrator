# The Intake Layer (step zero)

How a task gets INTO the system. This is the front door: you talking to the agent, before any code is written.

The rest of the product is agent-to-you (narration). This is you-to-agent. Same phone, same voice channel, both directions.

---

## 1. The core insight: intent vs precision

Voice is excellent at one thing and terrible at another, and coding needs both, but at different moments.

| | What it is | Voice is | Why |
|---|---|---|---|
| **Intent** | what to build, why, constraints, edge cases you are worried about, the shape of the thing | **excellent** | ~150 wpm spoken vs ~40 wpm typing on a phone. Rambling is fine, nuance survives, and you say things out loud you would never bother to type. |
| **Precision** | exact identifiers, file paths, API signatures, syntax | **terrible** | transcription mangles technical tokens. "user_id" becomes "user ID", library names get guessed. A misheard identifier looks confident and is worse than a blank. |

**Why this matters more than it sounds:** agents fail overwhelmingly at the intent stage, not the precision stage. A frontier model rarely fails because it could not write the syntax. It fails because it built the wrong thing, confidently. So voice attacks exactly the phase that is actually broken, and its weakness lands on the phase that was never the bottleneck.

That is the whole argument for a voice-first intake, and it is why this is genuinely better for people who code, not just a convenience.

## 2. The failure this prevents

Without an intake layer:
> You dictate for two minutes. The agent fills every gap with a guess. Twenty minutes later you get back confident, well-written, wrong code. You did not find out at minute 0 when it was free to fix. You found out at minute 20.

With an intake layer:
> You dictate for two minutes. The agent asks "the dashboard, do you mean the admin one or the user one?" You answer in three seconds. The ambiguity is dead before any code exists.

Clarity at the start is worth more than model choice later. This layer has more leverage than the routing table.

## 3. The design

**A three-beat loop, always the same:**

1. **You talk.** Ramble freely. No structure required. This is the part voice is good at.
2. **It interviews you.** It asks only the questions whose answers would change what gets built. Not a form, not twenty questions. If two readings of your request lead to materially different code, it asks. Otherwise it decides and moves on.
3. **It reflects a written brief back.** Short, plain English: what it is going to build, the assumptions it is making, what it will NOT touch. You confirm or correct. Only then does work begin.

**Step 3 is non-negotiable.** It is the precision backstop for a voice channel. Written text is where mis-transcriptions become visible: you SEE "user ID" when you said "user_id" and fix it in a second. Voice in, text confirmed, then build.

**The brief is also the artifact.** It becomes the task record, the thing the narration reports against, and what the PR-reviewer model checks the final code against ("did it build what the brief said?"). So it is not ceremony, it earns its place three more times downstream.

## 4. Showing, not just telling

The single biggest clarity gain after voice is being able to show something:
- a screenshot of the broken screen
- a photo of a whiteboard sketch
- a competitor's app: "like this, but simpler"

This is enormously more precise than any amount of describing, and it covers exactly the cases where words are weakest (layout, visual design, "it looks wrong"). Worth building. Note it needs image upload, NOT video calling.

## 5. What NOT to build

**Video calling.** The agent gains nothing from seeing your face. The valuable parts are voice (fast intent) and images (showing things). Video is theater and adds real infrastructure cost for zero capability. Skip it.

**Do not pitch this as "voice mode."** ChatGPT and Gemini have had voice for years; talking to an AI is not novel and will not turn heads on its own. The differentiator is the full loop, not the microphone:

> voice interview -> confirmed written brief -> async execution -> voice narration back

Nobody has that end to end. That is the wedge. "We have voice too" is not.

## 6. Build order

**V1 (cheap, most of the value):** voice or text in, clarifying questions, written brief, confirm, then build. This is an LLM conversation plus speech-to-text. No realtime infrastructure needed. Works over text equally well, which is a feature, not a compromise.

**V1.5:** image upload during intake (screenshot, whiteboard photo). High value per unit of effort.

**V2 (only if V1 proves the loop):** true live realtime call with screen share, using a realtime streaming API. This is where "point your camera at the whiteboard" magic lives, but it is also where the cost and complexity are. Do not build this first.

**Never:** video of the user's face.

## 7. The one-line version

Talk it out until the agent actually gets it, confirm the written brief, then let it build.
