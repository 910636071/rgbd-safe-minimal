# Title

Minimal Symbolic Pipelines for Constraint-Checked State Records

# Abstract

This study presents a compact computational artifact for comparing linear
symbolic methods over normalized records. The artifact maps synthetic cases to
trace stores, applies three deterministic baseline methods, checks candidate
records against finite constraint sets, and reports aggregate scores. The goal is
to make the relation between state construction, method selection, constraint
checking, and aggregate scoring explicit enough for direct reproduction.

# Introduction

- Motivation: small symbolic pipelines are useful for isolating state update and
  constraint checking behavior.
- Scope: normalized symbolic records, deterministic methods, finite constraint
  sets, aggregate score cards.
- Contribution: a minimal end-to-end artifact with 20 synthetic cases, three
  baseline methods, and repeated pilot runs.

# Methodology

## Pipeline

SyntheticCase -> TraceStore -> Baselines -> ConstraintCheck -> ScoreCard

## Minimal Formulation

Let:

- `r_t`: normalized symbolic record
- `s_t`: intermediate state
- `m`: method identifier
- `C`: constraint set
- `z_t`: checked output record
- `q`: aggregate score

The pipeline-level formulation is:

```text
s_t = F(s_{t-1}, r_t)
p_t = G(s_t, m)
z_t = H(p_t, C)
q = A({z_t})
```

## Baselines

- `summary_loop`: cumulative state selection.
- `template_grid`: position-indexed state selection.
- `symbolic_rule`: constraint-aware state selection.

# Ablation Study

- Compare all three baseline methods under the same 20 synthetic cases.
- Repeat each method ten times per case.
- Report aggregate score, pass rate, and average record count.

# Results

- Primary output: `outputs/summary_agg.csv`.
- Expected pilot size: 600 checked records.
- Metrics: `avg_q`, `pass_rate`, and `avg_record_count`.
- Initial aggregate values:
  - `summary_loop`: `avg_q=1.392134`, `pass_rate=0.100000`,
    `avg_record_count=6.850000`.
  - `template_grid`: `avg_q=0.793868`, `pass_rate=0.180000`,
    `avg_record_count=6.850000`.
  - `symbolic_rule`: `avg_q=0.917717`, `pass_rate=0.350000`,
    `avg_record_count=6.850000`.

# Discussion

- The artifact favors explicit state transitions and checked records.
- The comparison is intentionally small enough to inspect manually.
- The study is limited to deterministic normalized inputs and finite constraint
  lists.

# Reproducibility

Run:

```text
python -m scripts.run_pilot --cases data/cases_small.jsonl --runs 10 --methods summary_loop,template_grid,symbolic_rule --out outputs/pilot_runs.jsonl
python -m scripts.export_summary --input outputs/pilot_runs.jsonl --out outputs/summary_agg.csv
pytest -q
python -m scripts.term_scan
```
