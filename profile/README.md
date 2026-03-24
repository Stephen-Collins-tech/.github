# Stephen Collins.tech

> Studying how codebases actually behave.

We build tools and run analyses at the intersection of software complexity, codebase health, and AI-generated code. This org is where that work lives in the open.

---

<!-- DYNAMIC:START — updated daily via GitHub Actions -->
## Recent OSS Analyses

*Last updated: 2026-03-24 · [Full analysis archive](https://hotspots.dev/blog)*

| Repository | Highest Risk Function | Score |
|---|---|---|
| [colinhacks/zod](https://hotspots.dev/blog/2026-03-13-colinhacks-zod/) | `convertBaseSchema` | 21.3 |
| [FlowiseAI/Flowise](https://hotspots.dev/blog/2026-03-01-FlowiseAI-Flowise/) | `OpenAIAssistant` | 21.2 |
| [react-hook-form](https://hotspots.dev/blog/2026-03-17-react-hook-form-react-hook-form/) | `createFormControl` | 20.6 |
| [tldraw/tldraw](https://hotspots.dev/blog/2026-03-04-tldraw-tldraw/) | `Editor` | 19.3 |
| [styled-components](https://hotspots.dev/blog/2026-03-15-styled-components-styled-components/) | `sanitizeCSS` | 15.4 |

<!-- DYNAMIC:END -->

---

## What We Study

| Area | Description |
|---|---|
| **Structural complexity** | The 20% of functions causing 80% of bugs, incidents, and slowdowns |
| **AI-generated code** | How Copilot, Claude, and coding agents change codebase structure over time |
| **OSS health over time** | Longitudinal complexity drift in major open source projects |
| **Agentic codebases** | What happens to a codebase when agents write most of it |

---

## Recent Writing

- [What Happens When You Run Hotspots on 102,000 Functions](https://hotspots.dev/blog/2026-03-14-scale-testing-approximate-betweenness/) — stress-testing against VS Code, finding an O(N³) bug
- [AI Agents Can Pass Tests. They Still Can't Maintain Systems.](https://hotspots.dev/blog/ai-agents-cant-maintain-systems/) — how AI tools excel at writing but struggle with maintenance
- [AI Made Code Cheap. The Bottleneck Is Now Understanding Systems.](https://hotspots.dev/blog/ai-made-code-cheap/) — comprehension, not production, is now the engineering constraint

---

## Open Artifacts

- [`hall-of-fame/`](https://github.com/Stephen-Collins-tech/.github/tree/main/hall-of-fame) — Legendary gnarly functions from OSS history, analyzed
- [`ai-code-studies/`](https://github.com/Stephen-Collins-tech/.github/tree/main/ai-code-studies) — Research into AI-generated code patterns and failure cases

---

## The Tool

**[Hotspots](https://hotspots.dev)** finds the riskiest functions in any repo — the ones with high complexity and high churn that cause most of your bugs and slowdowns.

**macOS**
```sh
brew install Stephen-Collins-tech/tap/hotspots
hotspots analyze .
```

**Linux**
```sh
curl -fsSL https://raw.githubusercontent.com/Stephen-Collins-tech/hotspots/main/install.sh | sh
hotspots analyze .
```

---

## Also in this org

- **[ts-validator](https://github.com/Stephen-Collins-tech/ts-validator)** — Rust-based static analysis for runtime type safety in TypeScript
- **[intent-kit](https://github.com/Stephen-Collins-tech/intent-kit)** — Intent-driven workflow library for LLMs

---

[hotspots.dev](https://hotspots.dev) · [Blog](https://stephencollins.tech/blog) · [GitHub](https://github.com/Stephen-Collins-tech)
