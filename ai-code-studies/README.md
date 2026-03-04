# AI Code Studies

Research into how AI-generated and AI-assisted code changes the structural risk profile of codebases.

This is not about whether AI code "works." It usually does. This is about what happens to a codebase's long-term health when AI writes a growing percentage of it.

---

## Studies

| File | Topic |
|---|---|
| [`copilot-generated-patterns.md`](./copilot-generated-patterns.md) | Structural patterns observed in Copilot-generated code across OSS |
| [`agentic-codebase-risks.md`](./agentic-codebase-risks.md) | What happens to complexity when coding agents own large portions of a codebase |
| [`ai-refactor-failure-cases.md`](./ai-refactor-failure-cases.md) | Cases where AI-suggested refactors increased structural risk |

---

## The Core Question

LLMs optimize for local correctness — the generated code passes tests, satisfies the prompt, and looks reasonable in isolation.

But software risk is rarely local. It accumulates in:

- Coupling between modules
- Change frequency patterns over time
- The ratio of code that reviewers actually understand vs. code that exists

AI-generated code tends to increase all three. That is what we are studying here.

---

## Methodology

Analysis runs use [Hotspots](https://hotspots.dev) to compute per-file Lifetime Risk Scores (LRS) before and after AI-assisted changes. We look at repositories where AI contribution percentage can be estimated (via commit metadata, PR descriptions, or author self-reporting).

This is ongoing research. Findings are updated as new data becomes available.

---

*Research by [Stephen Collins.tech LLC](https://stephencollins.tech) · Powered by [Hotspots](https://hotspots.dev)*
