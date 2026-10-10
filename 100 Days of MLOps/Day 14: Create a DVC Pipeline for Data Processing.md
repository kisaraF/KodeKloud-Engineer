# Day 14: Create a DVC Pipeline for Data Processing

## Task Description

The xFusionCorp Industries ML team utilizes DVC pipelines to ensure the reproducibility of data processing. The fraud-detection project has the processing scripts and raw data in place but does not yet define a pipeline. Define a two-stage DVC pipeline so the data processing runs reproducibly from start to finish with dvc repro.

A project exists at `/root/code/fraud-detection/` with DVC initialised. The scripts are at `src/data/process_data.py` and `src/data/split_data.py`, and the raw input is at `data/raw/transactions.csv`. Do not modify the Python files or the input data.

**Acceptance criteria:**

- A `dvc.yaml` defines two stages:
    - `process_data` – runs `python3 src/data/process_data.py`; depends on `data/raw/transactions.csv` and `src/data/process_data.py`; produces `data/processed/clean_transactions.csv`.
    - `split_data` – runs `python3 src/data/split_data.py`; depends on `data/processed/clean_transactions.csv` (the upstream stage's output, so DVC chains the stages) and `src/data/split_data.py`; produces `data/processed/train.csv` and `data/processed/test.csv`.
- The pipeline has been reproduced so both stages execute in order and `dvc.lock` is written, and `dvc status` reports no stale stages.

> Use `python3` (not python) in the stage commands. Once the pipeline is valid, `dvc dag` prints the dependency graph showing how the two stages chain together.

## Solution

**1. We need to add the following yaml configuration first**

Check the comments regarding syntax explanations

```yaml
stages:
  process_data:
    cmd: python3 ./src/data/process_data.py
    deps: # what are the files that this action depends on
      - ./data/raw/transactions.csv
      - ./src/data/process_data.py
    outs: # output file produced. dvc will track anything in this field
      - ./data/processed/clean_transactions.csv
  split_data:
    cmd: python3 ./src/data/split_data.py
    deps:
      - ./src/data/split_data.py
      - ./data/processed/clean_transactions.csv # providing outs of previous stage will build a dep dag
    outs:
      - ./data/processed/train.csv
      - ./data/processed/test.csv
```

**2. Check the configuration is correctly done by running below commands**

```bash
dvc status # show a status message on each stage's outs and deps
dvc stage list # shows all stages and related info in the yaml
dvc dag # shows dep graph as below
```

![deps graph](https://res.cloudinary.com/divjxx9rs/image/upload/v1787503828/dvc_dep_graph_hwlmie.png)

**3. Run the pipeline**

To run the pipeline, you can use the below commannd

```bash
dvc repro
```

**4. Confirm if successful**

To confirm if it'ss successful there are few things to pay attention,
- It should create relavant output files at `data/`
- Running `dvc status` will output similar message as _"Data and pipelines are up to date."_
- A lock file as below would be created

```lock
schema: '2.0'
stages:
  process_data:
    cmd: python3 ./src/data/process_data.py
    deps:
    - path: ./data/raw/transactions.csv
      hash: md5
      md5: 262600809db02a8f3b97351c93c27784
      size: 549
    - path: ./src/data/process_data.py
      hash: md5
      md5: 0cdd7e3dbbe1e04f6b35bd38929b3b1f
      size: 263
    outs:
    - path: ./data/processed/clean_transactions.csv
      hash: md5
      md5: 16ee9b988c5a51591382422b56e11960
      size: 537
  split_data:
    cmd: python3 ./src/data/split_data.py
    deps:
    - path: ./data/processed/clean_transactions.csv
      hash: md5
      md5: 16ee9b988c5a51591382422b56e11960
      size: 537
    - path: ./src/data/split_data.py
      hash: md5
      md5: 42ce7c36686f56b713685884992fa160
      size: 363
    outs:
    - path: ./data/processed/test.csv
      hash: md5
      md5: 1a1fff319308070eb2bd682506d72faa
      size: 145
    - path: ./data/processed/train.csv
      hash: md5
      md5: 142467e5074926d5eb5e7154aa456c25
      size: 441
```

## Further Read

Read the dvc documentation page [here](https://doc.dvc.org/user-guide/project-structure/dvcyaml-files#stages) to learn more and to depth on `dvc.yaml` configurations
