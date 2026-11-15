# Async Agent Orchestrator

Design documents for a system that lets you build software from your phone. You dictate a task, it runs in a cloud workspace on whichever model is best suited to that kind of work, and it narrates every step in plain English, interrupting you only for decisions that are yours to make.

**Status:** design only. No code has been written yet.

## Documents

Read in this order.

| Document | Contents |
| --- | --- |
| [`what-im-building.md`](what-im-building.md) | The idea in plain words, before any architecture |
| [`async-agent-orchestrator-spec.md`](async-agent-orchestrator-spec.md) | The v1 spec: architecture, what to build versus rent, scope, and a first test |
| [`the-loop.md`](the-loop.md) | Show something small and real fast, and make being wrong cheap to correct |
| [`design-constitution.md`](design-constitution.md) | Rules every model follows, and the two cases where it must stop and ask |
| [`routing-table.md`](routing-table.md) | Starter table of which model handles which kind of task |
| [`risks-and-decisions.md`](risks-and-decisions.md) | Decisions made, risks reduced, and risks still open |

Model rankings in `routing-table.md` reflect the time they were written and change often.
