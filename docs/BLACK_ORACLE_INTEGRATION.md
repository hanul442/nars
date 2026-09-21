# BLACK ORACLE Integration

## Principle

NARS is an **evidence producer**. BLACK ORACLE is an **investment reasoning and decision system**.

NARS may say:
- this event is important;
- it is corroborated by these sources;
- it affects these entities;
- this is the current synthesis;
- confidence / conflict metadata looks like this.

NARS should not say:
- buy / sell;
- position size;
- strategy to run;
- order to execute.

## Proposed Evidence Package v1

```json
{
  "schema_version": "nars.evidence.v1",
  "event_id": "evt_...",
  "event_title": "...",
  "summary": "...",
  "published_at": "...",
  "last_updated_at": "...",
  "entities": [],
  "topics": [],
  "urgency": false,
  "nars_score": 0,
  "rank": 0,
  "score_components": {},
  "sources": [
    {
      "source_id": "...",
      "url": "...",
      "published_at": "...",
      "content_hash": "..."
    }
  ],
  "conflicts": [],
  "provenance": {
    "run_id": "...",
    "ranking_config_version": "v1"
  }
}
```

## BLACK ORACLE mapping

| NARS | BLACK ORACLE |
|---|---|
| Evidence Package | Evidence Store |
| Event / entities | Case candidate |
| Corroborating sources | Evidence links |
| Summary | Briefing input |
| Ranking metadata | Evidence priority, not trade conviction |
| Conflicts / uncertainty | Council / Red Team context |
| Provenance | Ledger trace |

## Integration rules

1. BLACK ORACLE must retain the original NARS `event_id`.
2. NARS score must not be treated as expected return or trade confidence.
3. Source URLs and hashes must survive downstream transformations.
4. Revisions should update the same event lineage rather than creating unrelated duplicates.
5. BLACK ORACLE may add investment-specific annotations but should not overwrite NARS provenance.

## Future interface

Preferred first implementation:
- versioned JSON file or HTTP endpoint;
- batch pull by time window;
- deterministic schema validation.

Streaming can be introduced later if latency requirements justify the operational complexity.
