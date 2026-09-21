# NARS Ranking Policy

## Baseline formula

The current baseline is:

```text
score = log(freq) * 12 + sqrt(freq) * 2 + urgent_bonus
urgent_bonus = 8 when urgent
```

Operational parameters:

| Parameter | Current value |
|---|---:|
| Cutoff | 20 |
| Previous cutoff | 16 |
| Maximum output | Top 80 |
| Urgent bonus | +8 |

`freq` represents corroborating / repeated event frequency after deduplication and event clustering. It must not be raw duplicate count from the same syndicated copy.

## Why the formula is separated from code

Weights and thresholds are a policy layer. They should be:
- versioned;
- configurable;
- replayable against historical data;
- evaluated through experiments before adoption.

The application code should expose score components rather than returning only a final number.

Example:

```json
{
  "frequency_component": 28.5,
  "urgent_component": 8,
  "total_score": 36.5,
  "cutoff": 20,
  "selected": true,
  "ranking_config_version": "v1"
}
```

## Required safeguards

### Duplicate inflation
Syndicated copies, URL variants, mirrored articles, and repeated newsletter excerpts must not artificially raise `freq`.

### Source concentration
Ten articles from one content network should not be treated as ten fully independent confirmations.

### Urgency leakage
The urgency flag must have an auditable trigger or classifier output. It should not become a catch-all manual boost.

### Topic crowding
A single dominant story can consume the Top 80. A later diversification layer may cap near-identical event families by topic or cluster.

## Evaluation metrics

Before changing weights, evaluate at least:
- selection precision;
- important-story miss rate;
- duplicate rate in final output;
- topic concentration;
- source concentration;
- ranking stability between runs;
- human override rate;
- downstream usefulness to BLACK ORACLE.

## Experiment protocol

Every scoring change should record:

```text
Hypothesis
→ Dataset / replay window
→ Baseline config
→ Challenger config
→ Metrics
→ Error analysis
→ Adopt / Reject
→ Config version
```

This keeps ranking development compatible with the broader Research → Test → Result → Adopt/Reject operating model used across HANUL projects.
