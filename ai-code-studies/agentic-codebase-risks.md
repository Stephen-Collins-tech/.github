# Agentic Codebase Risks

*What happens to a codebase's structure when coding agents write most of it.*

---

## The Shift

Copilot completes lines and functions. Coding agents — Cursor, Devin, Claude Code, Aider — complete features, refactor modules, and generate pull requests. The scope of AI contribution has grown from tokens to tasks.

This changes the risk profile of a codebase in ways that are not well understood yet. We're trying to understand them.

---

## Hypothesis 1: Agentic Code Has Lower Churn But Higher Coupling

Human developers tend to make small, incremental changes. Agents tend to make large, coherent changes — they rewrite a module rather than patching it. This could reduce churn per file but increase cross-file coupling if the agent introduces dependencies that a human reviewer doesn't catch.

**What to watch:** Files that have low commit frequency but high fan-in/fan-out. These might be agent-generated modules that "worked" and were never touched again — but are deeply coupled to the rest of the system.

---

## Hypothesis 2: Agents Reproduce the Patterns They Were Trained On

Coding agents are trained on existing code. If the training corpus skews toward specific architectural patterns (e.g., MVC, REST, particular ORM patterns), agents will reproduce those patterns even when they're not appropriate for the problem.

**Effect:** A codebase where agents write most of the code may converge toward a single architectural style that doesn't reflect the best solution for every component — it reflects what was most common in the training data.

This is the codebase equivalent of regression to the mean.

---

## Hypothesis 3: Agent Refactors Increase Hidden Complexity

When an agent refactors code, it optimizes for the metrics it can see: test pass rate, linter output, function length, naming consistency. It cannot optimize for the things that only emerge over time: change frequency patterns, reviewer comprehension, the cost of the next refactor.

**Result:** Agent refactors may look better by every local metric while making the codebase harder to change in practice. We've observed early evidence of this in the `ai-refactor-failure-cases.md` study.

---

## Hypothesis 4: The Review Problem

The fundamental risk of agentic code is not that it is wrong — it is that it produces large diffs that are difficult for humans to review meaningfully.

A 500-line PR from a human encodes months of decisions, trade-offs, and context. A 500-line PR from an agent encodes a prompt and a completion. The reviewer cannot tell which one they're looking at from the diff alone.

**Effect:** Review becomes a performance rather than a quality gate. The code merges because it looks right, not because a human understood it.

This is the most important structural risk of agentic development, and it doesn't show up in any code metric — it shows up in the next incident.

---

## What We're Building

A methodology for flagging agent-written code not because it's bad, but because it requires different review than human-written code:

- Higher structural scrutiny (coupling, cohesion)
- Explicit validation of edge case coverage
- Longer stabilization period before relying on it in critical paths

This is what Hotspots is being extended to support. Follow [hotspots.dev](https://hotspots.dev) for updates.

---

*Research by [Stephen Collins.tech LLC](https://stephencollins.tech) · Powered by [Hotspots](https://hotspots.dev)*
