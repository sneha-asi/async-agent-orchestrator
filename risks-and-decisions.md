# Risks and Decisions

Record of the hard calls made and the risks knowingly carried. Snapshot: 23 July 2026.

## Decisions locked

1. **API / pay-as-you-go, not subscriptions.** Access to models is via API (OpenRouter-style, users bring keys or pay-as-they-go), NOT by driving users' Claude Code / ChatGPT / Cursor subscriptions headlessly. Reason: headless orchestration of a subscription in our cloud likely violates provider ToS, and "same email" does not change that. Cost: we lose the "use what you already pay for" pitch. Benefit: legal and clean. This also moves token cost onto the user, which fixes unit economics.

2. **Byo-keys / pay-as-you-go economics.** Token cost sits with the user, so we only carry sandbox + orchestration cost. Caps our margin (the big cost is not ours to mark up) but is survivable. A flat all-you-can-use subscription would let heavy users bankrupt us, so we do not do that.

3. **Cursor excluded** except as a laptop-open-only special case for its own proprietary house models (unreachable any other way).

4. **Single official benchmark source: Arena.ai**, category-specific. Fallback to "latest most-powerful model" ONLY when no clear category winner exists.

5. **Non-dev mode never auto-merges to main**, even after user approval. A PR-reviewer model is a required extra gate.

## Risks reduced (mitigated, not eliminated)

- **Narration can misreport.** Mitigated by (a) faithful passthrough, we add no distortion, and (b) an independent low-cost watcher model that narrates rather than trusting the coder's self-report, plus the PR-reviewer catching "said it tested, didn't." Residual risk: the watcher itself can be wrong. Acceptable.
- **Cross-model parallel merge conflicts.** Mitigated by: parallel is free with the SAME model; different-model parallel goes through an integrator model that combines without overwriting. Residual: integrator quality unproven until real use.
- **Non-dev approval is weak safety.** Real protection is the mechanical guardrails + tests + PR-reviewer, not the user's tap. We rely on the mechanics, not the tap.

## Risks carried openly (not yet solved)

- **Benchmark-harness mismatch.** Arena.ai scores raw one-shot generations; we run multi-file edits in a repo through a harness. The best one-shot model may not be the best in-repo editor. Fallback-to-latest helps but is not rigorous. Carried as a known limitation.
- **Macro architectural coherence.** Different models across many tasks can drift the architecture. Plan: explore an architecture-keeper model + a continuous-coherence pipeline. Unsolved, validate with real users.
- **Market wedge.** The ideal user (technical enough to have a repo, wants phone-first async work) may be a thin slice. Tier-switching for single-model users widens it, but true demand is unknown until users arrive.
- **Key + code security.** Keys stored private in the cloud, we work on top without exposure. Still a real attack surface and trust ask; needs proper secrets management and isolation.

## The north star (what every decision serves)
One must be able to work without working: aware of what needs attention, free to do everything else.
