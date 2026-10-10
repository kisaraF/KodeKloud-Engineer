# Day 11: Track a Dataset with DVC

## Task Description

A teammate has added the transactions dataset to the xFusionCorp Industries fraud-detection repository. However, it was committed directly to Git rather than being tracked with DVC. Your task is to align the repository with the team standard, ensuring that every dataset under the `data/` directory is tracked by DVC instead of Git.


A project exists at `/root/code/fraud-detection/` with DVC already initialised. The dataset `data/raw/transactions.csv` is currently tracked by Git, and the team standard requires DVC to own it instead.

Acceptance criteria:

- Git no longer tracks the dataset, but the file remains on disk.
- The dataset is tracked by DVC instead: a `.dvc` pointer file exists and `data/raw/.gitignore` excludes the dataset itself.
- The new `.dvc` pointer and `.gitignore` are recorded in a Git commit with the message `Track transactions dataset with DVC`.

> Once tracking is moved to DVC, the DVC TRACKED section in the EXPLORER panel will list the dataset, confirming the extension recognises it as a DVC-managed file.

## Solution

**1. First remove the dataset from being tracked in git**

```bash
git rm -r --cached data/raw/transacctions.csv
git ls-files # to check if its not being tracked anymore
```

**2. Add to DVC**

```bash
dvc add data/raw/transactions.csv
cat data/raw/.gitignore # check if it git excluded from tracking list
cat data/raw/transactions.csv.dvc # check new changes metadata
```

**3. Commit to git the tracked changed**

```bash
git add . && git commit -m "Track transactions dataset with DVC"
```