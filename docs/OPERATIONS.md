# NARS Operations

## Scheduled outputs

### Daily — 08:00 KST
Purpose: deliver the highest-priority developments that materially changed since the previous daily window.

Recommended sections:
1. Top developments
2. Market / macro
3. Companies / sectors
4. AI / technology
5. Watchlist follow-ups
6. Newly urgent items
7. Evidence gaps / conflicting reports

### Weekly
Purpose: compress repetition and show what changed across the week.

Include:
- themes that persisted;
- stories whose importance increased or faded;
- unresolved claims;
- recurring entities / sectors;
- notable ranking misses or false positives;
- candidate adjustments for ranking experiments.

### Monthly
Purpose: system and trend review rather than a longer daily briefing.

Include:
- topic distribution;
- source distribution;
- event volume;
- selection rate;
- urgency rate;
- duplicate suppression rate;
- major persistent themes;
- ranking-policy performance;
- downstream feedback from BLACK ORACLE.

## Run discipline

Each run should persist:
- run ID;
- time window;
- configuration version;
- source counts;
- collected article count;
- unique article count;
- event cluster count;
- selected count;
- failure count;
- publish status.

## Failure policy

A partial source outage should not invalidate the entire run. NARS should:
1. mark the affected collector degraded;
2. continue with unaffected sources;
3. record missing coverage;
4. retry according to source policy;
5. expose the degraded state in the run report.

## Human intervention

Manual overrides should be rare and logged with:
- operator;
- timestamp;
- item / cluster;
- previous rank;
- new decision;
- reason.

The goal is not zero human judgment; it is traceable human judgment.
