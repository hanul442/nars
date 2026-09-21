# NARS Architecture

## 1. System boundary

NARS owns the lifecycle from **source ingestion** to **ranked evidence publication**. It does not own portfolio construction, strategy selection, order execution, or investment decisions.

## 2. Logical pipeline

```text
[Collectors]
    ↓
[Raw Item Store]
    ↓
[Normalizer]
    ↓
[Deduplication / Event Clustering]
    ↓
[Entity + Topic Enrichment]
    ↓
[Ranking Engine]
    ↓
[Summarizer]
    ↓
[Publisher]
    ├─ Daily Brief
    ├─ Weekly Brief
    ├─ Monthly Brief
    └─ BLACK ORACLE Evidence Feed
```

## 3. Stage contracts

### Collectors
Input-specific adapters fetch approved sources and emit a common raw item envelope.

Minimum fields:
- `source_id`
- `source_type`
- `url` or canonical locator
- `published_at`
- `retrieved_at`
- `title`
- `raw_text`
- `author` when available

### Normalizer
Produces canonical timestamps, normalized URLs, language, source identity, and stable content hashes.

### Deduplication / Event Clustering
Two layers are preferred:
1. exact / near-exact duplicate suppression;
2. semantic event clustering for multiple outlets covering the same underlying event.

A cluster should preserve every supporting article rather than replacing them with a single source.

### Enrichment
Adds:
- entities / tickers / organizations;
- topic tags;
- geography;
- event type;
- urgency flag;
- optional source-quality metadata.

### Ranking Engine
Consumes cluster features and configuration, then returns:
- total score;
- score components;
- cutoff decision;
- rank;
- reason codes.

### Summarizer
Produces short summaries that remain tied to their supporting evidence. It should distinguish:
- observed facts;
- attributed claims;
- unresolved or conflicting information.

### Publisher
Outputs human-readable briefings and machine-readable payloads.

## 4. Data objects

### Article
A single source item.

### Event Cluster
A group of articles describing the same underlying development.

### Ranked Event
An event cluster plus ranking features, score components, and selection status.

### Evidence Package
The downstream payload containing the event, summary, provenance, timestamps, entities, and NARS ranking metadata.

## 5. Reliability requirements

- deterministic content hashing;
- idempotent ingestion windows;
- retryable collectors;
- dead-letter path for malformed items;
- stage-level counters and structured logs;
- explicit schema versions for downstream payloads;
- reproducible ranking using a recorded config version.

## 6. Design decision

NARS should be implemented as a **modular pipeline first**, not as a multi-agent system by default. Agentic components can be introduced where judgment is genuinely useful—such as semantic clustering, entity resolution, or synthesis—but deterministic transforms and scoring should remain normal code where possible.
