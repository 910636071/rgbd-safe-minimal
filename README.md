# Minimal Symbolic Pipeline

This repository contains a small academic artifact for evaluating linear symbolic pipelines over normalized records.

The pipeline is:

```text
SyntheticCase -> TraceStore -> Baselines -> ConstraintCheck -> ScoreCard
```

all parameter values are toy values for reproducibility, not production values

## Files

- `data/cases_small.jsonl`: 20 synthetic cases.
- `scripts/run_pilot.py`: runs the pilot across cases, methods, and repeated passes.
- `scripts/export_summary.py`: writes aggregate CSV metrics.
- `scripts/term_scan.py`: scans repository text files for blocked names.
- `paper/outline.md`: paper structure and minimal mathematical formulation.

## Run

```powershell
python -m scripts.make_cases --out data/cases_small.jsonl
python -m scripts.run_pilot --cases data/cases_small.jsonl --runs 10 --methods summary_loop,template_grid,symbolic_rule --out outputs/pilot_runs.jsonl
python -m scripts.export_summary --input outputs/pilot_runs.jsonl --out outputs/summary_agg.csv
pytest -q
python -m scripts.term_scan
```

## Methods

`summary_loop` builds a cumulative state vector and selects the highest current state.

`template_grid` uses the trace position and pass index to choose among observed states.

`symbolic_rule` filters observed states with the case constraint lists before selecting a checked record.
