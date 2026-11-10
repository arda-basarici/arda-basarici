# Arda Başarıcı

**AI engineer — LLM systems, evaluation-first.** Mathematics background, ~3 years
shipping production games (20+ released titles), now building LLM applications end to
end: the evaluation data and its ground truth, the backend, the spend controls and the
deployment, alongside the model workflow. Deterministic where a model isn't needed,
measured against ground truth where one is, and open about the error that remains.

**Live:** [Leave Impact Agent](https://leave-agent.ardabasarici.dev)
([repo](https://github.com/arda-basarici/leave-impact-agent)) ·
[SteamLens](https://steamlens.ardabasarici.dev)
([repo](https://github.com/arda-basarici/steam-lens))

More about me: [ardabasarici.dev/about](https://ardabasarici.dev/about/) · reports &
write-ups: [ardabasarici.dev](https://ardabasarici.dev) ·
[LinkedIn](https://www.linkedin.com/in/ardabasarici)

---

## Leave Impact Agent — an LLM agent graded against an answer key generated with its world

An LLM agent reads an organization's HR system, issue tracker, calendar and documents,
works out which deadlines and meetings a leave affects and who can cover them, and
drafts a coverage plan for a person to approve. The systems are real instances of
Frappe HR, Jira and Google Calendar. The organization inside them is synthetic and was
generated together with its answer key.

**Live:** [leave-agent.ardabasarici.dev](https://leave-agent.ardabasarici.dev) ·
**Repo:** [leave-impact-agent](https://github.com/arda-basarici/leave-impact-agent) ·
**Report:** [Where the Single Call Stops](https://github.com/arda-basarici/leave-impact-agent/blob/main/report/where-the-single-call-stops.pdf)

- **A registered comparison, reported both ways:** frozen at a commit before the world
  it was measured on was generated. At the larger corpus size (about 42,500 tokens) the
  agent was correct whole on **24 of 30** scenarios where one call shown every document
  got **14** (33 points, interval 20 to 47 at 95 percent). That call, Claude Haiku 4.5
  under an 8,192-token output maximum, ended at the maximum on 24 of its 30 calls. At the
  smaller size (about 10,500 tokens) the single call scored higher, 30 to 24.
- **The model writes no verdict:** it reads and states facts with their quotes, and
  deterministic rules compose every claim and the plan from the facts that are admitted.
- **One event log is the authority:** recovery after a crash, the audit trail, the run's
  export and the public timeline are all reads of the same log in PostgreSQL. A run
  takes its spend reservation against a ledger in the transaction that admits it, and
  live runs are capped at five a day.

**Covers:** LLM agents · evaluation against constructed ground truth · deployment on AWS with spend control
**Stack:** Python, LangGraph, Amazon Bedrock (Claude Haiku 4.5), PostgreSQL, FastAPI, Docker, GitHub Actions, AWS

---

## SteamLens — a live LLM product that publishes its own error rate

Type a game name, watch an AI investigate its Steam reviews, get a report where every
claim is traceable to source reviews, a measured sampling tolerance, or a published
error rate.

**Live:** [steamlens.ardabasarici.dev](https://steamlens.ardabasarici.dev) ·
**Repo:** [steam-lens](https://github.com/arda-basarici/steam-lens)

- **Evaluation:** the labeller is measured against a human-adjudicated 250-review gold
  set (assist-model drafts, every label ruled by hand before any candidate model was
  scored). The deployed instrument was re-certified at **F1 0.801 (95% CI 0.752–0.844)**
  on 2026-09-24, after the provider re-pointed the requested model id and the drift
  watch saw nothing; the original certification was 0.766 (0.713–0.811). The watch now
  declares the expected version and aborts on the first foreign response.
- **Grounding:** quotes are verified verbatim at write time and numerals at compose
  time: **0 non-verbatim quotes among 163,842 stored evidence spans**, a construction
  invariant: about 2.9% of attempted quotes failed verification and were nulled at write
  time. Residual attribution error, 11.6% by human audit on the prior model and pending
  re-measurement, is disclosed inside the product.
- **Production, solo:** a 135,260-review census labelled for $3.80 at the time (one
  refusal logged) · CI evaluation gates · approval-gated deploys with rollback ·
  per-report spend admission · public ops dashboard.

**Covers:** LLM evaluation · grounding / hallucination control · production deployment & ops
**Stack:** Python, FastAPI, SQLite, DeepSeek + Gemini behind one client seam, Docker/GHCR, GitHub Actions, Cloudflare

---

## platform — how the projects are operated

The operational layer under both applications: the shared Cloudflare edge, the two
hosts (a VPS and an AWS EC2 instance), the cloud resources, and the per-project wiring
that joins an application to them. One repository, owned by no application.

**Repo:** [platform](https://github.com/arda-basarici/platform) ·
**Best single read:** the [rebuild-and-restore runbook](https://github.com/arda-basarici/platform/blob/main/runbooks/box-rebuild.md)

- **Infrastructure as code, adopted on zero-diff plans:** every DNS record of the zone and
  its deliberately set edge settings and security rules, and every AWS resource under the
  agent, in Terraform; the VPS rebuildable from a blank host by a five-role Ansible play
  with an acceptance script.
- **A recovery drill, dated:** on 2026-08-30 a rebuild-and-restore drill took a blank
  host and a blank control node to SteamLens serving restored production data in
  **about an hour**, against a throwaway host. A monthly automated restore check runs
  since; the alarm path was verified by forcing the alarm, which found and fixed a
  silent SNS policy refusal.
- **Secrets by policy:** the store follows workload identity (SSM, SOPS + age, OIDC);
  production secret values kept out of Terraform state with a state-pull proof; every SOPS
  file also encrypted to a recovery identity whose private key lives in the password vault,
  outside the workstation.

**Covers:** infrastructure as code · operations & recovery · secrets handling
**Stack:** Terraform, Ansible, Cloudflare, AWS (EC2, SSM, IAM OIDC, CloudWatch), Caddy, Docker, SOPS/age

---

## The investigations behind it

Four earlier projects, each a concrete question answered in a written technical report.
The evaluation discipline is the same across all of them; only the system under test
changes.

| Project | Question | Finding |
|---|---|---|
| [blackjack-rl](https://github.com/arda-basarici/blackjack-rl) | Can RL rediscover provably-optimal decisions? | A tabular agent audited against provable basic strategy (~93% of cells rediscovered, residual traced), then a DQN on the same task (~82% naive, ~91% with a stabilizer stack). A bet-sizer never finds Kelly; an oracle control and two falsification experiments show why. Built on [blackjack-sim](https://github.com/arda-basarici/blackjack-sim), a from-scratch Monte Carlo engine validated on the published 0.45% house edge over ~90M hands. |
| [pathfinding-ml](https://github.com/arda-basarici/pathfinding-ml) | Can a learned heuristic beat Manhattan distance? | A gradient-boosted cost-to-go heuristic: at the report's 50/50 maze mix, about 17% fewer node expansions at a 0.2% mean optimality gap on held-out mazes, found on the far side of a Simpson's reversal that made the pooled result look like a wash. In this study the training-distribution composition, not the model, governed the outcome. |
| [steam-reviews](https://github.com/arda-basarici/steam-reviews) | What does "85% positive" actually measure? | A resumable pipeline (298,553 reviews, 30 languages, schema contracts gating every promotion) and four findings each required to reproduce inside individual games; one 42-point pooled effect failed that bar and was discarded. |

**Stack across these:** Python, PyTorch, scikit-learn, NumPy, pandas, Parquet, pandera, scipy
