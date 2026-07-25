# Routing Table (starter)

Maps task category to the model to use. Source of truth: **Arena.ai** category arenas. This is the fastest-changing file in the project (four frontier models landed in the six weeks before this snapshot), so treat every row as "as of the date below," and refresh from Arena.ai.

**Snapshot date: 23 July 2026.**

## How routing actually works
1. Detect the task category (below).
2. Look up the ranked models for that category from Arena.ai.
3. **Constrain to the models THIS user has access to** (from onboarding).
4. Pick the highest-ranked one the user actually has. That is the model. Fall back down the list if needed.
5. If cost mode is on, prefer the cheapest model within a small score band of the top, not always the #1.

So the table below is "ideal pick, then alternatives," and the system silently drops to the best available alternative per user.

## The table

| Task category | Ideal pick now | Alternatives (fallback order) | Arena.ai arena |
|---|---|---|---|
| Frontend / UI / design | Kimi K3 | Claude Fable 5, Claude Opus 4.8 | Frontend Code Arena |
| Screenshot / image to code | Claude 4.6 (Opus/Sonnet) | Gemini 3.x, Kimi K3 | Image-to-WebDev |
| Full web app (frontend + backend) | Claude Fable 5 | Claude Opus 4.7-thinking, Qwen 3.7 Max | WebDev Arena |
| General coding / bug-fix / refactor | Claude Opus 4.8 | Claude Opus 4.7-thinking, GPT-5.6 Sol, GLM-5.1 | Coding Arena |
| Complex reasoning / architecture / hard tasks | Claude Opus 4.8 | GPT-5.5 Pro, Gemini 3.1 Pro | Overall + Expert / Hard Prompts |
| Math | Claude Opus 4.8 | Qwen 3.7 Max | Math |
| High-volume boilerplate / cheap codegen | GLM-5.1 | Gemini 3.5 Flash, Kimi K3 (open weight) | Coding Arena, cost-weighted |

## Notes
- **Frontend is the clearest specialist win:** Kimi K3 jumped to #1 in Frontend Code Arena (1679), ranking first in 6 of 7 frontend sub-domains, above the Claude models. This is the strongest case in the whole table for routing away from a default model.
- **Coding/reasoning is Anthropic-led right now** (Opus 4.8 / 4.7-thinking), but GPT-5.6 Sol and GLM-5.1 are close enough to be real alternatives, and are the natural fallback when a user lacks Opus access.
- **Open-weight models (GLM, Kimi) are the cost play:** competitive scores, much cheaper, ideal for high-volume or cost-mode users.
- Rankings cluster tightly (top models often within ~50 ELO). When the top two are within the band and one is much cheaper or one the user already has, prefer that one. Do not over-optimize on tiny score gaps.

## Refresh
Re-pull Arena.ai category leaders on a schedule (e.g. weekly). The manual override layer handles launch-day claims that beat the leaderboard by a week or two.
