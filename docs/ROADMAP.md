# NARS Roadmap

## Phase 0 — Repository bootstrap
**Status: in progress**

- [x] define system boundary
- [x] document baseline ranking policy
- [x] document operating cadence
- [x] define BLACK ORACLE integration contract
- [ ] merge repository bootstrap PR

## Phase 1 — Vertical slice
Goal: prove the full path with one source.

- [ ] project runtime / dependency setup
- [ ] one collector
- [ ] canonical Article schema
- [ ] content hashing
- [ ] simple duplicate suppression
- [ ] minimal event grouping
- [ ] ranking config loader
- [ ] baseline score implementation
- [ ] summary output
- [ ] Evidence Package v1 JSON
- [ ] unit tests

**Exit criterion:** one source can run end-to-end repeatedly without duplicate outputs.

## Phase 2 — Multi-source evidence
- [ ] multiple collectors
- [ ] source registry
- [ ] semantic event clustering
- [ ] entity / ticker enrichment
- [ ] source concentration safeguards
- [ ] structured provenance
- [ ] integration tests

## Phase 3 — Scheduled briefings
- [ ] Daily 08:00 KST run
- [ ] weekly aggregation
- [ ] monthly system review
- [ ] run ledger
- [ ] failure / degraded-source reporting
- [ ] publish adapters

## Phase 4 — Ranking R&D
- [ ] historical replay dataset
- [ ] baseline vs challenger configs
- [ ] important-story miss review
- [ ] duplicate / concentration metrics
- [ ] topic diversification experiments
- [ ] Adopt / Reject decision log

## Phase 5 — BLACK ORACLE production integration
- [ ] schema version enforcement
- [ ] authenticated delivery
- [ ] event lineage updates
- [ ] downstream acknowledgements
- [ ] observability / SLOs

## Immediate next build

The first coding PR should implement only this:

```text
1 collector
→ Article schema
→ dedupe
→ rank
→ summary
→ nars.evidence.v1 JSON
```

Avoid prematurely building dashboards or a large agent organization before this path is measurable and testable.
