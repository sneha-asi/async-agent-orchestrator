# Async Agent Orchestrator, v1 Spec

## One-line pitch
Dictate coding tasks from your phone, the system routes each to the best model, runs it in the cloud while you do other things, and narrates every step in plain English, interrupting you only for real decisions.

## The core idea
You never open your laptop. The code lives in one repo inside a cloud sandbox. Different models work on it through their own harnesses, all committing to that same repo. The repo is the sync layer, so harnesses never need to talk to each other. You stay aware of every step by ear, and steer by voice.

## Architecture (the whole thing in order)

1. **Onboard.** Collect three things from the user:
   - Which model subscriptions / API accesses they have (only enable those).
   - Serial or parallel tasks (default: serial).
   - Whether they need any model that exists ONLY inside Cursor.

2. **Model access.** Direct API keys where the user has them, OpenRouter as the fallback aggregator for open models (GLM, Kimi, DeepSeek, Qwen, etc.).

3. **Sandbox per user.** One persistent cloud workspace that spins up on demand and sleeps when idle. Rent this, do not build it: E2B, Daytona, Modal, or GitHub Codespaces. Holds the repo, runs the harnesses, stays alive while you sleep.

4. **Three harnesses cover every model** (you do NOT need one per model):
   - Claude Code (Agent SDK) runs Opus / Sonnet. Headless.
   - Codex (open-source CLI) runs GPT models. Headless.
   - An open harness (Aider / OpenHands / Cline-core) runs everything else: Kimi, GLM, DeepSeek, Qwen. Headless, points at any endpoint.
   - Cursor: excluded, EXCEPT as a laptop-open-only special case for its own proprietary house models, since those cannot be reached any other way.

5. **Routing.** A rule table maps task category to model, constrained to the models the user actually has. Fed LIVE from ONE official source.
   - Source of truth: **Arena.ai** (formerly LMArena / LMSYS Chatbot Arena). Chosen because it is category-specific ("best model for WHAT"): separate arenas for Frontend Code, WebDev, Image-to-WebDev, general coding, math, etc. Neutral third-party, human-preference, continuously updated. It is the source the frontend claim (Kimi #1 in frontend, beating the Claude models) was officially released from.
   - Why not others: SWE-bench Verified is the most rigorous single benchmark but only measures agentic bug-fixing (one category, fails the "best for what" need). LLM Stats / Vellum are aggregators, one layer removed from the source and mixing methodologies.
   - Thin manual override only for launch-day claims not yet reflected in Arena.ai.
   - Fallback: when there is no clear category winner for a task, route to the latest most-powerful model of the moment (tracked by news / launch announcements). This is a fallback ONLY, not the default.
   - Tier-switching within one harness: even a single-model user benefits. Use a cheap tier for boilerplate and an expensive tier for the hard parts, to save cost. Routing has value for everyone, not just multi-model users.
   - Example output: frontend/design -> whichever tops Frontend Code Arena, complex refactor/reasoning -> whichever tops the coding arena.
   - Starter table with today's category winners filled in: see routing-table.md.

6. **Execution + safety (automatic for everyone, dev and non-dev alike).** Before any agent writes a line, it is injected with the **Design Constitution** (see design-constitution.md), a distilled ruleset from Parnas (information hiding), Ousterhout (deep modules / complexity), and Software Engineering at Google (health over time), plus the safety presets. This is what makes design quality consistent across models (Opus, Kimi, GLM all follow the same rules). The chosen harness works on a task branch and auto-commits there. Safety rules and the two hard stops are also enforced mechanically by the orchestrator, not left to the model's goodwill. Guarantee: the system only makes the code better, never breaks it, and every user knows what is happening regardless of skill level.

7. **Narration layer (the actual product).** Two things happen at once.
   - Every moment is sorted into: **needs your decision** -> interrupt with a question (which model, merge to main, delete anything, spend money); or **just needs narrating** -> stream to phone in plain English while you keep doing other things (editing a file, running a test, fixing an import).
   - Narration has two depths, same completeness: **dev mode** goes deeper (files changed, functions, the why); **non-dev mode** gives the mains in plain outcomes ("added a login screen, tested it, works"). Non-dev mode is NOT less information, it is the same full journey at a higher altitude. Nobody is ever left unaware.
   - **Independent watcher model.** Narration does NOT come from the coding model reporting on itself (it could misreport). A separate low-cost model watches the coding model and narrates independently. Narration is a light task, so a cheap model is enough, and independence means a coder's misreport gets caught, not repeated.

## Multi-model quality pipeline (the helper models)
Beyond the model that writes code, the system runs light supporting models:
- **Watcher / narrator** (low cost): observes the coding model and produces the plain-English narration independently. See step 7.
- **PR-reviewer:** after the coding model finishes, a second model reviews the change before anything merges. In non-dev mode, NEVER auto-merge to main even after user approval, the reviewer is a required extra gate. Also partly patches narration risk (catches "said it tested, didn't").
- **Integrator** (cross-model parallel only): when different models work in parallel, a final model makes sense of everything together, no overwriting, respects what each model did, commits only the best combined result. Parallel with the SAME model does not need this.
- **Architecture-keeper** (exploratory): a model that tries to make sense of what is happening across tasks and keeps the base architecture stable over time (macro coherence). Micro coherence is handled by the Design Constitution; macro coherence across many tasks and models is an open problem to explore, not a solved one.

8. **Close the loop.** On task completion, ping the phone with a plain-English summary. You approve merge to main by voice. Then it asks what is next and keeps pinging so you finish the day's work aware of every step.

## What is genuinely yours to build vs what already exists
- Already exists: model access (OpenRouter), sandboxes (E2B/Daytona), headless harnesses (Claude Code SDK, Codex, Aider/OpenHands).
- Yours, and missing from the market: the benchmark-driven routing table + the decision-vs-narration layer + the phone-first voice loop.

## User journey scope
Must work from 1 user to 100 to many. Every user, dev or non-dev, always knows exactly what is being done to their code (via the two narration depths). System design never breaks their code, only improves it (via the preset safety rules).

## Deliberately deferred to v2
- Mobile diff-viewer (v1 uses summary + "approve / not yet", not full code review on phone).
- Parallel tasks with cross-model merge-conflict handling (advanced users only).

## Suggested first test (before building the full thing)
Pick one task category you do a lot (e.g. frontend for your hackathon work). Wire ONLY that path end to end on your phone: dictate -> one sandbox -> one harness -> branch commit -> plain-English narration -> approve. Feel whether "aware of every step by ear while multitasking" actually works. That one test teaches more than months of building the router.
