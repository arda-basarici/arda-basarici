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

## Leave Impact Agent — an LLM agent that investigates a leave and drafts a coverage plan

An LLM agent reads an organization's HR system, issue tracker, calendar and documents,
works out which deadlines and meetings a leave affects and who can cover them, and
drafts a coverage plan for a person to approve. The systems are real instances of
Frappe HR, Jira and Google Calendar. The organization inside them is synthetic and was
generated together with its answer key.

**Live:** [leave-agent.ardabasarici.dev](https://leave-agent.ardabasarici.dev) ·
**Repo:** [leave-impact-agent](https://github.com/arda-basarici/leave-impact-agent) ·
**Report:** [Where the Single Call Stops](https://github.com/arda-basarici/leave-impact-agent/blob/main/report/where-the-single-call-stops.pdf)

- **A registered comparison at two corpus sizes:** frozen at a commit before the world
  it was measured on was generated. At the larger size the agent answered **24 of 30**
  scenarios completely correctly where the full-context baseline, one call shown every
  document, answered **14** (33 points, 95% interval 20 to 47). Most of the baseline's
  calls, Claude Haiku 4.5 under an 8,192-token output maximum, stopped at that maximum.
  At the smaller size the full-context baseline scored higher, 30 to 24.
  [The result in full](https://github.com/arda-basarici/leave-impact-agent#the-result).
- **The model writes no verdict:** it states facts with their quotes, and deterministic
  rules compose every claim and the plan from the admitted facts.
- **One event log is the authority:** recovery after a crash, the audit trail, the run's
  export and the public timeline are all reads of the same log in PostgreSQL. Spend is
  admitted before it is made, and live runs are capped at five a day.

**Stack:** Python, LangGraph, Amazon Bedrock (Claude Haiku 4.5), PostgreSQL, FastAPI, Docker, GitHub Actions, AWS

---

## SteamLens — a live LLM product that publishes its own error rate

Type a game's name and get an evidence-backed report of what players praise and
complain about, every finding tied to source reviews or to statistics computed from
them, each share with its sampling interval. The system's own error rates are published
beside its claims, inside the product.

**Live:** [steamlens.ardabasarici.dev](https://steamlens.ardabasarici.dev) ·
**Repo:** [steam-lens](https://github.com/arda-basarici/steam-lens)

- **Evaluation:** the labeler is measured against a 250-review gold set, every label
  adjudicated by a single annotator before any candidate model was scored. The deployed
  labeler was re-certified at **F1 0.801 (95% CI 0.752–0.844)** on 2026-09-24, after the
  provider re-pointed the requested model id.
  [Every measured claim](https://github.com/arda-basarici/steam-lens#measured-not-asserted).
- **Grounding:** quotes are verified verbatim at write time and numerals at compose
  time: **0 non-verbatim quotes among 163,842 stored evidence spans**. Attribution errors
  remain, and their measured rate is disclosed inside the product.
- **Production, solo:** a 135,260-review census labeled for $3.80 at the time · CI
  evaluation gates · approval-gated deploys with rollback · per-report spend admission ·
  public ops dashboard.

**Stack:** Python, FastAPI, SQLite, DeepSeek + Gemini behind one client seam, Docker/GHCR, GitHub Actions, Cloudflare

---

## platform — how the projects are operated

The operational layer under both applications: the shared Cloudflare edge, the two
hosts (a VPS and an EC2 instance) and the cloud resources. One repository, owned by no
application.

**Repo:** [platform](https://github.com/arda-basarici/platform) ·
**Best single read:** the [rebuild-and-restore runbook](https://github.com/arda-basarici/platform/blob/main/runbooks/box-rebuild.md)

- **Infrastructure as code, adopted on zero-diff plans:** the Cloudflare zone and every
  AWS resource under the agent in Terraform; the VPS rebuildable from a blank host by an
  Ansible play with an acceptance script.
  [What it holds](https://github.com/arda-basarici/platform#what-it-holds).
- **Recovery tested:** on 2026-08-30 a rebuild-and-restore drill took a blank host and a
  blank control node to SteamLens serving restored production data in **about an hour**,
  against a throwaway host. A monthly automated restore check has run since then, and
  the alarm path was proven by forcing the alarm.
- **Secrets by policy:** the store follows workload identity (SSM, SOPS + age, OIDC);
  production secret values are kept out of Terraform state; every SOPS file is also
  encrypted to a recovery identity held outside the workstation.

**Stack:** Terraform, Ansible, Cloudflare, AWS (EC2, SSM, IAM OIDC, CloudWatch), Caddy, Docker, SOPS/age

---

## Earlier investigations

Each answers one question in a written technical report.

| Project | Question | Finding |
|---|---|---|
| [blackjack-rl](https://github.com/arda-basarici/blackjack-rl), on the [blackjack-sim](https://github.com/arda-basarici/blackjack-sim) engine | Can RL rediscover provably optimal play? | A tabular agent rediscovers ~93% of basic strategy. A learned bettor never finds Kelly betting, and controlled experiments show why: the counting edge sits below the per-hand noise. |
| [pathfinding-ml](https://github.com/arda-basarici/pathfinding-ml) | Can a learned heuristic beat Manhattan distance? | About 17% fewer node expansions at a ~0.2% optimality gap on held-out mazes. The gain depends on the maze types: trained on one and tested on the other, it fails. |
| [steam-reviews](https://github.com/arda-basarici/steam-reviews) | What does "85% positive" measure? | A resumable pipeline over 298,553 reviews in 30 languages. Four patterns reproduced inside individual games; a pooled Early Access gap (44.8% vs 87%) vanished under that test. |

**Stack across these:** Python, PyTorch, scikit-learn, NumPy, pandas, Parquet, pandera, scipy
