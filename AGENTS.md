# Project memory and working instructions

This file provides persistent project context for coding agents working in this
repository. Keep it concise and update it when durable project decisions change.
Verify current files and Git state before relying on dated observations. Do not
store credentials, private data, or full conversation transcripts here.

## Project purpose

Build a financial fraud detection model using the synthetic PaySim dataset.
The intended outputs for a transaction are a fraud probability, a risk level,
and an explanation of the factors contributing to that classification.

## Current project state

Last verified: 2026-09-17.

- `README.md` describes the objectives, dataset, evaluation concepts, and initial
  exploration results.
- `test.ipynb` currently loads the dataset with pandas and prints its shape,
  sample rows, columns, types, missing values, and fraud class distribution.
  Model training and risk scoring are not implemented yet.
- The recorded dataset has 6,362,620 rows and 11 columns, with 8,213 fraud cases.
  These are recorded exploration results, not a fresh validation of the CSV.
- There is no dependency manifest or automated test suite yet. The notebook
  currently requires pandas and a Jupyter-compatible Python environment.
- The current working environment is Windows with PowerShell.

## Dataset and Git

- Dataset source: https://www.kaggle.com/datasets/ealaxi/paysim1
- Local dataset: `PS_20174392719_1491204439457_log.csv` in the project root.
- Keep the original dataset intact and excluded from ordinary Git commits.
  Its filename is already listed in `.gitignore`.
- A previous push was rejected because the roughly 470.67 MB CSV was committed.
  Do not re-add it with `git add -f`. Ignoring a file alone does not remove it
  from existing commits. Check history if the large-file rejection returns.
- Expected remote: `https://github.com/infernoxthecat/ML-Financial-Fraud-Detector.git`.
  The working branch was `main` when this file was created. Verify the remote
  and branch before pushing; successful publication has not been verified.
- Read notebook source separately from outputs when inspecting it. Avoid
  dumping the full CSV into a terminal or loading it repeatedly for small checks.

## Modeling guidance

- The target is `isFraud`. Keep it out of input features.
- Account for the severe class imbalance. Compare a baseline with a
  class-weighted model, as proposed in the README.
- Evaluate precision, recall, F1, and precision-recall performance; do not rely
  on accuracy alone. Explain the false-positive/false-negative tradeoff when
  choosing thresholds.
- Split data before fitting preprocessing or resampling. Keep test data out
  of model fitting and threshold selection, and use reproducible random seeds
  where applicable.
- Check whether each feature would be available when a transaction is scored.
  Explicitly assess balance fields and `isFlaggedFraud` for leakage or reliance
  on an existing detector before including them.
- Treat risk bands and explanation methods as design decisions still to be
  made; do not describe them as implemented features.

## Working style and validation

- Explain changes and Git commands in plain language so the user can learn
  alongside the work.
- Follow the existing notebook workflow unless the task calls for a different
  structure. Avoid unrelated refactors and unnecessary dependencies.
- For documentation-only changes, inspect the resulting diff; model execution
  is unnecessary.
- For notebook changes, validate notebook structure and run the affected code
  when practical. State whether validation used a sample or the full dataset,
  and report any steps that could not be run.
- Keep this file aligned with implemented behavior. Record durable decisions
  and useful setup instructions rather than a running task log.
