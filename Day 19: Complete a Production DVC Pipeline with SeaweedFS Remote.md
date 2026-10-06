# Day 19: Complete a Production DVC Pipeline with SeaweedFS Remote

## Task Description

Complete the xFusionCorp Industries fraud-detection production DVC pipeline. Three stages are already wired in `dvc.yaml`, two remain, and the pipeline must finish as a reproducible, SeaweedFS-backed, v1.0-tagged release.

A project exists at `/root/code/ml-pipeline/` with Git and DVC initialised. The `params.yaml` is in place and the `.dvc/config` is pre-configured to push to the SeaweedFS bucket `dvc-storage` at `http://localhost:8333`.

The `ingest`, `validate`, and `preprocess` stages are already declared in `dvc.yaml`, but one of them is misconfigured and prevents `dvc repro` from completing — run dvc repro to see it fail. The two scripts for the remaining stages are pre-staged at `/root/code/ml-pipeline/scripts-staging/train.py` and `scripts-staging/evaluate.py`, and belong in `scripts/`.

Acceptance criteria:

- The misconfigured existing stage is corrected so `dvc repro` can complete.
- Two further stages are declared in `dvc.yaml`:
  - `train` – Depends on the preprocessed dataset and `scripts/train.py`; reads `n_estimators`, `max_depth`, `test_size`, and `random_seed` from `params.yaml`; outputs `models/model.pkl` and `data/processed/test_split.csv`; declares `metrics.json` as a DVC metric with `cache: false`.
  - `evaluate` – Depends on `models/model.pkl`, `data/processed/test_split.csv`, and `scripts/evaluate.py`; outputs `reports/evaluation.json` declared with `cache: false`.
- The full pipeline has been reproduced, the cache pushed to the SeaweedFS remote, and the current state tagged `v1.0`.
- Every change is committed to Git so the release is fully captured.

> Open the SeaweedFS Filer button at the top of the lab and navigate to `/buckets/dvc-storage/` to confirm that the bucket holds the pushed artefacts under the `files/md5/...` layout.

## Solution

**1. DVC configuration should be like below**
- `dvc repro` before config edit (test run) fails due to "preprocess" stage's output file name typo
- Every output that is not cached (default is cache:true) will not be uploaded to SeaweedFS remote filer when `dvc push` is executing

```yaml
stages:
  ingest:
    cmd: python3 scripts/ingest.py
    deps:
      - scripts/ingest.py
      - data/raw/data.csv

  validate:
    cmd: python3 scripts/validate.py
    deps:
      - data/raw/data.csv
      - scripts/validate.py
    outs:
      - reports/validation.json:
          cache: false

  preprocess:
    cmd: python3 scripts/preprocess.py
    deps:
      - data/raw/data.csv
      - scripts/preprocess.py
    outs:
      - data/processed/clean.csv
  
  train:
    cmd: python3 scripts/train.py
    params:
      - n_estimators
      - max_depth
      - test_size
      - random_seed
    deps: 
      - data/processed/clean.csv
      - scripts/train.py
    outs:
      - models/model.pkl
      - data/processed/test_split.csv
    metrics:
      - metrics.json:
          cache: false
  
  evaluate:
    cmd: python3 scripts/evaluate.py
    deps:
      - scripts/evaluate.py
      - models/model.pkl
      - data/processed/test_split.csv
    outs:
      - reports/evaluation.json:
            cache: false
```

**2. Copy all the missing scripts from the staging**

```bash
cp scripts-stagin/evaluate.py scripts-staging/train.py scripts/
```

**3. Reproduce the pipeline**

```bash
dvc repro
```
This should run without issues

**4. Push the cached files to remote**
- Pushing the cached files to SeaweedFS remote
```bash
dvc push
```
- SeaweedFS filer bucket corresponds to the correct file by showing first two characters of the MD5 hash in the folder and the rest in the file name inside the folder
![dvc remote](https://res.cloudinary.com/divjxx9rs/image/upload/v1791311029/DVC_Remote_n9nf9h.png)

**5. Add dvc.lock to git and make a git tag**

```bash
git add . && git commit -m "release v1"
git tag -a v1.0 -m "version 1"
```