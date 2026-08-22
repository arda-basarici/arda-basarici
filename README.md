# Arda Başarıcı

**AI engineer — LLM systems, evaluation-first.** Mathematics background, ~3 years
shipping production games (20+ released titles), now building LLM systems that can
state how well they work: deterministic wherever a model isn't needed, measured
against ground truth wherever one is, and open about the error that remains.

More about me: [ardabasarici.dev/about](https://ardabasarici.dev/about/) · reports &
write-ups: [ardabasarici.dev](https://ardabasarici.dev) ·
[LinkedIn](https://www.linkedin.com/in/ardabasarici)

---

## SteamLens — a live LLM product that publishes its own error rate

Type a game name, watch an AI investigate its Steam reviews, get a report where every
claim carries its quoted evidence — and the product cites its own measured accuracy.

**Live:** [steamlens.ardabasarici.dev](https://steamlens.ardabasarici.dev) ·
**Repo:** [steam-lens](https://github.com/arda-basarici/steam-lens)

- **Evaluation:** the production labeller measured against a human-adjudicated
  250-review gold set, labelled before any model output existed — **F1 0.766
  (95% CI 0.713–0.811)**; a cross-family LLM judge, calibrated on the same gold set,
  extends the check beyond it.
- **Grounding:** quote and numeral verification before anything publishes —
  **0 non-verbatim quotes among 163,842 stored evidence spans**; residual attribution
  error measured (11.6%) and disclosed inside the product.
- **Production, solo:** a 135,260-review census labelled for $3.80 · CI evaluation
  gates · approval-gated deploys with rollback · per-report spend admission · public
  ops dashboard · 746 tests.

**Covers:** LLM evaluation · grounding / hallucination control · production deployment & ops
**Stack:** Python, FastAPI, SQLite, multi-provider LLM APIs, Docker/GHCR, GitHub Actions, Cloudflare

---

## The investigations behind it

Four earlier projects, each a concrete question answered in a written technical report.
The evaluation discipline is the same across all of them; only the system under test
changes.

### [blackjack-rl](https://github.com/arda-basarici/blackjack-rl) — can RL rediscover provably-optimal decisions?

A tabular agent audited against provable basic strategy (~93% of cells rediscovered,
residual traced), a DQN on the same task (~82–92% across seeds, gap isolated by
ablation) — and a bet-sizer that never finds Kelly, with the *why* proven by two
controlled experiments. The negative result is the published finding.

**Covers:** reinforcement learning · experiment design & falsification · reproducibility
**Stack:** Python, PyTorch, NumPy

### [pathfinding-ml](https://github.com/arda-basarici/pathfinding-ml) — can a learned heuristic beat Manhattan distance?

A gradient-boosted cost-to-go heuristic: 17.3% fewer node expansions at a 0.2% mean
optimality gap on held-out mazes — found on the far side of a Simpson's reversal that
made the pooled result look like a wash. Training-distribution composition, not the
model, governs the outcome.

**Covers:** supervised ML · leakage-safe evaluation · confound detection & distribution shift
**Stack:** Python, scikit-learn, NumPy

### [steam-reviews](https://github.com/arda-basarici/steam-reviews) — what does "85% positive" actually measure?

A resumable pipeline (298,553 reviews, 30 languages, schema contracts gating every
promotion) and four findings each required to reproduce inside individual games — one
42-point pooled effect failed that bar and is reported as a discard.

**Covers:** data pipeline engineering · schema contracts · within-group statistical confirmation
**Stack:** Python, pandas, Parquet, pandera, scipy

### [blackjack-sim](https://github.com/arda-basarici/blackjack-sim) — the validated foundation

The from-scratch Monte Carlo engine the RL work audits against: ~90M hands, validated
on the published 0.45% house edge before anything was built on it.

**Covers:** simulation · validation against known ground truth
**Stack:** Python, NumPy

---

## Currently building

An agent system whose world is constructed so its answers can be checked: real
organizational tools (HRMS, issue tracker, calendar) populated by a synthetic-org
generator that doubles as the golden-dataset generator — every scenario ships with its
answer key, so the agent's investigation is graded, not admired. Public as it grows.
