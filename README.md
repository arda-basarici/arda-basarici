# Arda Başarıcı

Software engineer with a mathematics background, moving into **AI / ML engineering**.
I spent the past several years shipping production software — 25+ released games, real
users, real deadlines — and I bring that discipline to building AI systems: architecture
that stays maintainable, tests that make meaningful claims, and results that can be
regenerated from a seed.

Rather than treating projects as demonstrations of tools, I treat them as **engineering
investigations**: a concrete question, strong baselines, controlled experiments, and a
technical report that explains not only what worked, but why — including the hypotheses
that failed.

## How I work

- **Measurement before optimization.** A number with no baseline and no error bar isn't a
  result yet.
- **Test explanations, not just models.** A conclusion is earned by the controlled
  experiment that could have falsified it.
- **Negative results are findings.** My strongest published result is a demonstration of
  _why_ learning fails on a problem — reported as the finding it is.
- **Reproducible by construction.** Experiments carry their config, seed, and code
  version — enough context to rerun and independently verify a finding.
- **Complexity must earn its place.** The right tool for the question — no framework,
  layer, or model added unless it demonstrably buys something.

None of this is specific to one model family. Evaluation discipline is the same job whether
the system under test is a Q-table, a gradient-boosted tree, or an LLM pipeline — which is
exactly where I'm taking it (see below).

## The work so far — the evidence

My public work to date spans simulation, data engineering, and machine learning. One line
each; the write-ups carry the full story:

- **[blackjack-rl](https://github.com/arda-basarici/blackjack-rl)** — can RL rediscover
  provably-optimal decisions? The capstone inverts: the learned bettor never finds Kelly,
  and the project _proves why_ — the edge is real but sits below the noise it must be
  learned from. Structure beats end-to-end learning on a sub-noise signal.
- **[steam-reviews](https://github.com/arda-basarici/steam-reviews)** — what does "85%
  positive" measure? A resumable data pipeline (298k reviews, 30 languages, contract-
  validated) and four findings forced to reproduce _inside individual games_ — plus a
  chapter on the claims the data refused to support.
- **[pathfinding-ml](https://github.com/arda-basarici/pathfinding-ml)** — a learned A\*
  heuristic beats Manhattan (~17% fewer nodes, ~0.2% optimality gap), found on the far side
  of a Simpson's reversal. The real lesson: the training distribution governs the outcome,
  not the model.
- **[blackjack-sim](https://github.com/arda-basarici/blackjack-sim)** — the from-scratch,
  self-validating Monte Carlo engine the RL work audits against (~90M hands, validated on
  the published house edge).

Every project ships with a technical report and full design/architecture documentation.

📄 **Write-ups, reports & code:** [arda-basarici.github.io](https://arda-basarici.github.io)

## Current focus

**AI engineering** — building AI systems end to end: design, orchestration, and above all
_measuring what they actually do_. Built publicly, like everything above.
