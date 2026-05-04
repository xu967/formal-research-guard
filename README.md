# Formal Research Guard

Formal Research Guard is a Codex skill for research projects where experiments, paper results, code changes, and rollback records must be reproducible and publication-ready.

It helps Codex avoid treating temporary runs as formal research results. The skill is intentionally project-agnostic: it does not assume a specific dataset, model, paper, lab, repository layout, or change-log filename.

## What This Skill Enforces

The skill asks Codex to treat research outputs as formal records by default.

It requires Codex to:

- Read project-local rule files before acting.
- Avoid running partial experiments unless the user explicitly asks for debugging.
- Confirm the paper table, method row, dataset, checkpoint, protocol, metrics, seeds, logs, outputs, and rollback method before formal experiments.
- Keep timestamps on formal results and project changes.
- Avoid writing incomplete, diagnostic, or short-schedule results into papers.
- Record code, paper, data, environment, experiment, and rollback changes in the project change log when one exists.

## When To Use

Use this skill when Codex works on:

- Academic research repositories.
- Machine learning training or evaluation runs.
- Result tables and paper numbers.
- LaTeX paper edits.
- Baselines, ablations, fine-tuning, or evaluation scripts.
- Checkpoints, datasets, or environment changes.
- Experiment tracking and rollback records.

## What It Does Not Do

This skill does not provide a training framework, dataset loader, paper template, or experiment runner.

It is a process guard. It helps Codex decide whether an action or result is formal enough to be recorded, compared, or written into a paper.

## Installation

Clone this repository into your Codex skills directory:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone git@github.com:xu967/formal-research-guard.git "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
```

If you prefer HTTPS:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/xu967/formal-research-guard.git "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
```

After installation, start a new Codex session so the skill metadata can be discovered.

## Repository Contents

```text
formal-research-guard/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── formal-records.md
```

`SKILL.md` contains the trigger metadata and core workflow.

`agents/openai.yaml` contains UI-facing skill metadata.

`references/formal-records.md` contains a compact experiment checklist, change-log template, and paper-result gate.

## Expected Workflow

When the skill triggers, Codex should first inspect the current project for local rule files, such as:

```text
Init.md
AGENTS.md
README.md
EXPERIMENT_PLAN.md
EXPERIMENT_TRACKER.md
CHANGELOG.md
*_变更与回滚记录.md
```

Project-local rules take priority over the generic rules in this skill.

Before running a formal experiment, Codex should identify:

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

If required formal conditions are missing, Codex should state the missing items instead of starting the run.

## Non-Formal Runs

The following runs must not be treated as formal results unless the user explicitly requests debugging:

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

These runs may be useful for engineering checks, but they should not be written into paper tables or formal result paragraphs.

## Timestamp Rule

Every formal result and every project change must carry a timestamp.

Default format:

```text
YYYYMMDD-HHMM
```

The timestamp should appear in at least one durable location:

```text
change log section title
experiment log filename
output directory
result JSON, CSV, or JSONL filename
checkpoint directory
paper compilation record
rollback entry
```

## Validation

If you have the Codex skill-creator validation script available, validate the skill with:

```bash
python /path/to/skill-creator/scripts/quick_validate.py /path/to/formal-research-guard
```

For example, on a system where the built-in skill-creator skill is installed under `~/.codex/skills/.system`:

```bash
python ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
```

Expected output:

```text
Skill is valid!
```

## Updating

To update an installed copy:

```bash
cd "${CODEX_HOME:-$HOME/.codex}/skills/formal-research-guard"
git pull
```

## Design Notes

This skill is intentionally generic. Keep project-specific instructions in the target project, not in this skill.

Good project-local files include:

- `Init.md`
- `AGENTS.md`
- `EXPERIMENT_PLAN.md`
- `EXPERIMENT_TRACKER.md`
- `CHANGELOG.md`
- project-specific change and rollback logs

The skill should point Codex to those files and enforce reproducibility rules around them.
