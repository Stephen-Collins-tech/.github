# .github Org Marketing — Task Plan

## Goal

Transform the stephencollins.tech GitHub org page from blank to a living engineering research hub. Target audience: engineers who land here after seeing Hotspots, a blog post, or a trending artifact.

Principle: **Marketing for engineers should look like engineering.**

---

## Open Questions (resolve before Phase 2)

- [ ] What is the GitHub org name? (needed for links in README)
- [ ] Does Hotspots have a public API? Or do we build the pipeline as part of this?
- [ ] Hall of fame examples — user-selected or AI-curated from known gnarly OSS?
- [ ] ~~`hotspots.dev/api/random` Easter egg~~ — endpoint doesn't exist yet, removed from README for now
- [x] Install script URL: `https://raw.githubusercontent.com/Stephen-Collins-tech/hotspots/main/install.sh` (not hotspots.dev/install.sh)

---

## Phase 1 — Foundation (can build now, no pipeline needed)

### 1.1 `profile/README.md`
The org landing page. First and most important artifact.

- [ ] Lead with a one-liner that frames the org as a research lab, not a company
- [ ] Add a static "Recent Analyses" table (manually seeded, later auto-updated)
- [ ] Add a "What we study" section (complexity, AI-generated code, risk patterns)
- [ ] Single frictionless CTA: Homebrew (macOS) + install script (Linux)
- [ ] Add placeholder markers for dynamic sections (injected by Actions later)
- [ ] Link out to: hotspots.dev, blog, hall-of-fame, open-source-breakdowns

### 1.2 `hall-of-fame/`
Manually curated. High viral potential — engineers star and share this.

- [ ] `README.md` — index of entries with one-line descriptions
- [ ] 3–5 initial entries, each in their own folder:
  - `god-functions/linux-scheduler/` — the CFS scheduler in kernel/sched/fair.c
  - `god-functions/sqlite-tokenizer/` — sqlite3GetToken()
  - `complexity-monsters/left-pad/` — the 11-line function that broke the internet
  - `complexity-monsters/openssl-heartbleed/` — the bug hiding in plain sight
  - `ai-generated/` — reserved for AI-written code case studies (Phase 3)
- [ ] Each entry contains: `analysis.md` (what it does, why it's complex, LRS if applicable)

### 1.3 `ai-code-studies/`
Connects Hotspots to the AI coding wave. Strong positioning.

- [ ] `README.md` — framing: we study how AI changes codebase structure
- [ ] `copilot-generated-patterns.md` — initial observations (can be qualitative)
- [ ] `agentic-codebase-risks.md` — what happens when agents write most of the code
- [ ] `ai-refactor-failure-cases.md` — cases where AI refactors increase complexity

### 1.4 GitHub Actions — Scaffolding
Makes the org feel maintained and automated even before the pipeline is live.

- [ ] `.github/workflows/update-readme.yml` — stub that runs daily, placeholder logic
- [ ] `.github/workflows/run-analysis.yml` — stub for Hotspots pipeline trigger
- [ ] Add workflow status badges to `profile/README.md`

---

## Phase 2 — Data Engine (needs Hotspots pipeline)

### 2.1 Dynamic README Updates
- [ ] Wire `update-readme.yml` to actually run Hotspots across trending GitHub repos
- [ ] Script to inject results into README between HTML comment markers
- [ ] "Trending Repo Risk Index" table — top 10 repos by risk score, updates daily
- [ ] "Latest Analyses" table — last 5 runs with links

### 2.2 `open-source-breakdowns/`
Auto-generated per-repo analysis folders. The content engine.

- [ ] Pipeline generates per-repo folder with: `analysis.md`, `risk-map.png`, `metrics.json`
- [ ] Seed with 5–10 high-profile repos: kubernetes, redis, postgres, react, rustc, sqlite
- [ ] `README.md` index that links all breakdowns with top-level risk scores

### 2.3 `code-complexity-dataset/`
The research artifact. Makes you look like you're doing real work (because you are).

- [ ] `datasets/2026-q1/` — initial batch of OSS repos analyzed
- [ ] `reports/risk-distribution.md` — aggregate stats across the dataset
- [ ] `reports/largest-hotspots.md` — top files by LRS across all analyzed repos
- [ ] `reports/ai-generated-patterns.md` — patterns detected in AI-written code
- [ ] `README.md` — framing as an open dataset, encourage contributions

---

## Phase 3 — Viral Artifacts

### 3.1 Top 100 Riskiest OSS Codebases
The HN-bait piece. Requires Phase 2 pipeline running at scale.

- [ ] Run Hotspots across top 500 GitHub repos by stars
- [ ] `top-100-riskiest/README.md` — ranked table with repo, highest-risk file, LRS, notes
- [ ] Publish as standalone artifact + blog post + social
- [ ] Update quarterly

### 3.2 Easter Eggs
Engineers share clever interactive things.

- [ ] `curl https://hotspots.dev/api/random` — returns a random OSS repo analysis (needs API built first)
- [ ] Hidden section in README: "Run this:" with the curl command
- [ ] Consider: a small CLI demo in the README that engineers can copy-paste

### 3.3 Quarterly "State of Code Complexity" Report
Long-term positioning as the authority on codebase health.

- [ ] `reports/2026-q1-state-of-complexity.md`
- [ ] Sections: trending risk patterns, AI code growth, most improved repos, hall of fame additions
- [ ] Designed to be linked, quoted, and shared

---

## Success Metrics

- GitHub org stars (currently: 0)
- Inbound links to hotspots.dev from GitHub
- README views (via traffic insights)
- Hall of fame / dataset repo stars
- HN / Reddit threads referencing the org artifacts
