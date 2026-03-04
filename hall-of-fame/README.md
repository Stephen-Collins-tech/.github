# Hall of Fame

A curated collection of legendary functions, modules, and code artifacts from OSS history — analyzed for structural complexity, historical impact, and what they teach us about software risk.

These are not bugs. They are the real thing: code that worked, shipped, scaled, and occasionally broke the world.

Each entry includes a structural analysis using [Hotspots](https://hotspots.dev) metrics where applicable.

---

## God Functions

Functions that do too much, know too much, and somehow still run in production at massive scale.

| Entry | Repo | Why It's Here |
|---|---|---|
| [`linux-scheduler/`](./god-functions/linux-scheduler/) | `torvalds/linux` | 4,000 lines of CFS scheduling logic. The most complex function most software ever touches. |
| [`sqlite-tokenizer/`](./god-functions/sqlite-tokenizer/) | `sqlite/sqlite` | A tokenizer that is also a parser, a state machine, and a case study in deliberate complexity. |

---

## Complexity Monsters

Not a single function — entire files or modules where complexity became load-bearing.

| Entry | Repo | Why It's Here |
|---|---|---|
| [`left-pad/`](./complexity-monsters/left-pad/) | `left-pad/left-pad` | 11 lines that took down thousands of builds. A lesson in hidden dependency risk. |
| [`heartbleed/`](./complexity-monsters/heartbleed/) | `openssl/openssl` | The bug that hid in plain sight. Structural complexity as a security surface. |

---

## AI Generated

*(Coming soon)* — Case studies of AI-written code that passed review but carried structural risk invisible to humans.

---

Contributions welcome. If you know a function that belongs here, open a PR with an `analysis.md`.
