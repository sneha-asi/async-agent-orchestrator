# What I am trying to build

In plain words, in my own framing. This is the origin of the idea, before any architecture.

---

## The basic thing

I want to build software from my phone.

I dictate what I want. It gets built while I do other things. I never open my laptop. I could be on a call with AirPods in, walking around, or asleep, and the work still moves. By the end of the day, things are done.

I am the one with the clarity. I know what I am trying to build. The models are the hands.

## The main part: I must know what is happening

This matters more to me than anything else.

I want to be aware of each and every step. Not a summary at the end. Not a wall of code I cannot read on a phone. Every step, explained in simple words, while it happens.

- It should tell me what it is doing as it does it.
- It should ping me. Reminders, notifications, so I stay with it through the day.
- When something finishes, it tells me and asks what is next.
- If I am a developer, tell me deeper: files, functions, why.
- If I am not a developer, tell me the main things, but still tell me everything. Just at a higher level. Nobody should be left in the dark.
- Do not add your own spin. Tell me what the model actually said and did. If it is wrong, that is on the model, not on a summary that got creative.
- Better: put a separate cheap model on top whose only job is to watch and tell me. Do not let the model doing the work be the one reporting on itself.

## It must never break my code

Assume the person using this might not be a developer. Assume they cannot catch a mistake themselves. So the system has to protect them by default.

- Safety rules preset for every project, the first-principles ones.
- Never merge to main, even after I approve. Put another model in between that reviews it like a PR first.
- Non-developer mode needs even more guardrails, not fewer.
- My API keys are never exposed. They stay private in the cloud and the work happens on top of that.

The promise is simple: it only makes my code better. It never breaks it.

## Use the best model for the job

I do not want to be loyal to one tool. I want whatever is actually best right now.

- Connect everything: Claude, Codex, Kimi, open models like GLM.
- I tell it what I have access to. It only uses those.
- It picks the right model per task, based on officially released benchmarks, one reliable source, not ten.
- I mean the raw models, not the tools around them.
- If there is no clear winner for a task, use the latest most powerful model out right now.
- Even if I only have one model, switching tiers matters. Cheap model for easy parts, expensive for the hard parts, so I do not burn money.

## How the work actually runs

- Each model runs through its own harness, because that is where it performs best.
- One repo is the single place everything lives. All the harnesses read and write to it, so they never need to talk to each other.
- Serial or parallel is my choice, because parallel costs more.
- Parallel is fine when it is the same model. When different models work at once, a final model puts it all together at the end. No overwriting. Respect what each one did. Push only the best result.
- Something should keep the overall architecture stable over time, not just each individual change.
- It runs in a sandbox in the cloud, not on my machine, so my laptop stays closed.

## Money

Pay as you go. Users tell us what they have, they pay us, we pay the providers. Like OpenRouter.

## Who it is for

It has to work for one user, then a hundred, then many. Developers and non-developers both. Either way, they always know exactly what is being done to their code.

## The one line

I want to work without working. Aware of what needs my attention, free to do everything else.

---

## What I noticed writing this down

The things I kept coming back to on my own, without being asked, were all the same two things:

**Tell me everything, in words I understand. And do not break what I have.**

Not the model routing. That was the part I explained the most, but awareness and safety are the parts I actually cared about. That is the real spine of this idea.
