# Formal Research Records

## Pre-Experiment Checklist

Use this checklist before starting a research experiment:

```text
timestamp:
paper target:
method:
baselines:
dataset:
checkpoint:
training protocol:
fine-tuning protocol:
evaluation protocol:
seeds:
metrics:
output path:
log path:
expected table row:
rollback method:
```

If any required field is unknown, do not run the experiment as a formal result.

## Change Log Template

Use this template when the project has a change log:

```markdown
## YYYYMMDD-HHMM Short Change Title

Purpose:

Modified files:

Commands:

Outputs:

Validation:

Warnings:

Rollback:
```

## Paper Result Gate

Before adding a number to a paper, verify:

```text
same dataset across compared methods
same training budget across compared methods
same fine-tuning schedule across compared methods
same evaluation script across compared methods
same seed count across compared methods
complete baseline coverage
timestamped logs and outputs
```

If the answer is no for any line, describe the result as internal debugging only and do not write it into the paper.
