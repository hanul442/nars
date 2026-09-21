# NARS

NARS is the news automation and ranking pipeline for HANUL projects. Its job is to turn a high-volume stream of news and research inputs into a small, prioritized, traceable evidence feed that can be consumed by humans and downstream systems such as BLACK ORACLE.

## Core responsibilities

1. **Collect** — ingest news, reports, newsletters, and other approved sources.
2. **Normalize** — standardize metadata, timestamps, entities, topics, and source identity.
3. **Deduplicate / Cluster** — merge repeated coverage of the same event while preserving source provenance.
4. **Rank** — prioritize items using an explicit scoring policy.
5. **Summarize** — produce concise, evidence-linked summaries.
6. **Publish** — emit Daily / Weekly / Monthly briefings and machine-readable evidence feeds.
7. **Audit** — retain why an item was selected, how it was scored, and which sources supported it.

## Current ranking baseline

```text
score = log(freq) * 12 + sqrt(freq) * 2 + urgent_bonus
urgent_bonus = 8 when urgent
```

- Current cutoff: **20**
- Previous cutoff: 16
- Maximum ranked output: **Top 80**
- The ranking policy is configuration-driven so weights and thresholds can be tested without rewriting the pipeline.

See [docs/RANKING.md](docs/RANKING.md).

## Operating cadence

- **Daily briefing:** 08:00 KST
- **Weekly briefing:** aggregate material changes, repeated themes, and unresolved developments
- **Monthly briefing:** higher-level trend, topic, source, and signal review

See [docs/OPERATIONS.md](docs/OPERATIONS.md).

## BLACK ORACLE relationship

NARS should not make portfolio decisions. It supplies normalized evidence and event context to BLACK ORACLE.

```text
Sources
  ↓
NARS Collect → Normalize → Cluster → Rank → Summarize
  ↓
Evidence Feed
  ↓
BLACK ORACLE Evidence Store / Case / Council / Briefing / Ledger
```

This separation keeps news ingestion and ranking independently testable while allowing BLACK ORACLE to consume NARS as one evidence producer.

See [docs/BLACK_ORACLE_INTEGRATION.md](docs/BLACK_ORACLE_INTEGRATION.md).

## Target repository layout

```text
nars/
├─ README.md
├─ config/
│  └─ ranking.example.yaml
├─ docs/
│  ├─ ARCHITECTURE.md
│  ├─ RANKING.md
│  ├─ OPERATIONS.md
│  ├─ BLACK_ORACLE_INTEGRATION.md
│  └─ ROADMAP.md
├─ src/                 # implementation will be introduced by feature PRs
├─ tests/               # unit / integration / regression tests
└─ legacy/              # unrelated or historical files preserved for traceability
```

## Engineering principles

- **Evidence first:** every summary must retain source provenance.
- **Configurable ranking:** scoring changes belong in config and experiments, not hidden constants.
- **No silent drops:** rejected or filtered items should have a reason code.
- **Idempotent runs:** rerunning the same window should not create duplicate events.
- **Observable pipeline:** each stage should expose counts, failures, latency, and selection rates.
- **Human-auditable outputs:** a reader should be able to reconstruct why a story reached the briefing.
- **Downstream neutrality:** NARS ranks information importance; it does not decide trades.

## Status

**Repository bootstrap / architecture definition.**  
The next implementation milestone is an end-to-end vertical slice:

```text
1 source → normalized article → clustered event → ranked event → summary → JSON evidence feed
```

Track implementation sequencing in [docs/ROADMAP.md](docs/ROADMAP.md).
