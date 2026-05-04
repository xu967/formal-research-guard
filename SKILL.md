---
name: formal-research-guard
description: Enforce formal research project safeguards for academic experiments, machine learning training and evaluation, LaTeX paper updates, result tables, code changes, dataset changes, environment changes, timestamps, reproducibility records, and rollback logging. Use when Codex works on research code, runs or plans experiments, summarizes results, edits paper text, fills tables, changes training or evaluation scripts, manages checkpoints or datasets, or records project changes where results must be publication-ready and reproducible.
---

# Formal Research Guard

## Core Rule

Treat research outputs as formal records by default. Do not run or write partial experiments, debug-only results, or temporary numbers into a paper unless the user explicitly asks for a debugging pass.

## Required First Step

Before making changes or running experiments, inspect the current project for local rule files and follow them. Project-local rules override this generic skill.

Look for files such as:

```text
Init.md
AGENTS.md
README.md
EXPERIMENT_PLAN.md
EXPERIMENT_TRACKER.md
CHANGELOG.md
*_变更与回滚记录.md
```

If a change log exists, record every code, paper, data, environment, experiment, and rollback change there.

## Formal Experiment Gate

Before starting an experiment, identify:

```text
paper table or section
method row
baseline coverage
dataset
checkpoint
training protocol
fine-tuning protocol
evaluation protocol
seed list
metrics
output path
log path
timestamp
rollback method
```

Do not start the experiment if required formal conditions are missing. Instead, state the missing items and wait for the user decision.

Avoid treating these as formal results unless the user explicitly requested a debug run:

```text
smoke
pilot
diagnostic
short schedule
single seed only
temporary checkpoint
synthetic substitute
partial baseline set
```

## Paper Updates

Only add results to a paper when the project protocol is complete.

Before inserting numbers into a paper table or paragraph, confirm:

```text
all compared methods use the same protocol
all required baselines are complete
all required seeds are complete
metrics come from the formal evaluation script
logs, outputs, and checkpoints are preserved
the result has a timestamped record
```

Remove debug, pilot, short-schedule, or incomplete results if they were written into a paper by mistake.

## Timestamp Rules

Every formal result and every project change must carry a timestamp.

Use this timestamp format by default:

```text
YYYYMMDD-HHMM
```

Place the timestamp in at least one durable location:

```text
change log section title
experiment log filename
output directory
result JSON, CSV, or JSONL filename
checkpoint directory
paper compilation record
rollback entry
```

Results or changes without timestamps must not be treated as formal records.

## Change Logging

For any formal change, record:

```text
timestamp
purpose
modified files
commands
output files
validation result
known warnings
rollback method
```

For paper edits, include compilation commands and whether the build passed. For code edits, include tests or the reason tests were not run.

## Additional Reference

For a compact checklist and wording templates, read:

```text
references/formal-records.md
```
