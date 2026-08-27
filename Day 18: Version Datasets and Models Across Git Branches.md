# Day 18: Version Datasets and Models Across Git Branches

## Task Description

The xFusionCorp Industries MLOps team versions datasets and models on separate Git branches so it can reproduce and roll between versions cleanly. Tag the current state as `v1.0`, create a `v2-improved` branch built on a newer dataset (which retrains the model), and confirm that switching back restores the original data and model.


A project exists at `/root/code/fraud-detection/` with a working DVC pipeline (it processes the data and trains a model) and the baseline `data/raw/transactions`.csv already tracked.

An improved dataset has been pre-staged at `/root/code/fraud-detection/data/raw/transactions_v2.csv` and is visible in the file explorer. Do not delete this file.

**Acceptance criteria:**

- On the main branch, the current state is tagged `v1.0`.
- A branch named `v2-improved` holds the v2 state: the tracked dataset carries the contents of the v2 file (re-tracked with DVC), the pipeline has been re-run so `models/model.pkl` is retrained and versioned alongside the dataset, and the changes are committed.
- Back on the main branch, the v1 dataset and model are restored on disk, matching the hashes recorded by the `v1.0` tag.

>The DVC extension's DVC TRACKED section in the EXPLORER panel reflects the tracked dataset and model for the branch you currently have checked out. To compare the exact hashes recorded on each branch, use `git show <ref>:dvc.lock` or `dvc status`.

## Solution

**1. Create a git tag**

```bash
git tag -a v1.0 -m "some_message"
```

To learn more about git tagging, read this [guide](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

**2. Create a new branch and copy v2 dataset content to transactions**

```bash
git checkout -b v2-improved
cd data/raw
cp transactions_v2.csv transactions.csv
```

**3. Then add the dataset to dvc for tracking and reproduce the pipeline**

```bash
dvc add transactions.csv
cd ../..
dvc repro
```

This will change the datasets associated with the pipeline and the `models.pkl` file as well. We can confirm this through git status and lock file

**4. Final step**

Then commit these changes before switching back to `main` branch

```bash
git add . && git commit -m "new changes"
```

Then use the `dvc checkout` command to bring back the dvc index to `main` branch's one. Otherwise we'd have datasets like `train`, `test` still as in the v2 branch's. And `dvc status` should confirm this.