# Day 15: Parameterize a DVC Pipeline

## Task Description

The xFusionCorp Industries ML team manages model hyperparameters using `params.yaml`, enabling experiments to be conducted without altering the code. In the fraud-detection project, the `train` stage retrieves the `n_estimators` parameter from `params.yaml`, but this parameter is not declared to DVC, which means that changing its value does not initiate retraining. Integrate the parameter into the pipeline and illustrate the concept of parameter-driven reproducibility.


A project exists at `/root/code/fraud-detection/` with a three-stage DVC pipeline (`process_data`, `split_data`, `train`) and a `params.yaml` declaring `n_estimators: 100`. `src/models/train.py` already reads `n_estimators` from `params.yaml`. Do not modify the Python files.

The `train` stage in `dvc.yaml` currently has no `params:` section, so DVC does not track `n_estimators` — changing it would not re-run the stage.

Acceptance criteria:

- The `train` stage in `dvc.yaml` lists `n_estimators` under a `params:` section, and the pipeline has been reproduced.
- Parameter-driven retraining is demonstrated: with `n_estimators` changed to a different value (for example `200`), re-running the pipeline re-executes only the `train` stage, records the new value in `dvc.lock`, and regenerates `models/model.pkl`.

> dvc params diff reports changes to the tracked parameter values across Git commits, which is useful when comparing experiments.

## Solution

All we have to change is the `dvc.yaml` file's one line. Add the params name under the `train` stage so that it will correctly substitute the value to python script at run time. 

```yaml
stages:
    .
    .
    .

  train:
    cmd: python3 src/models/train.py
    params:
      - n_estimators # add like this
    deps:
      - data/processed/train.csv
      - src/models/train.py
    outs:
      - models/model.pkl
```

When adding parameters, you can add a specific parameter name from `params.yaml` file. Or you can configure the name in a few different ways to cater your requirements. To learn more read [this](https://doc.dvc.org/command-reference/params) official documentation.

Running the pipeline with `dvc repro` will use the param value defined at the run time of the relavant stage. Using the `dvc repro <stage-name>` you can re-run a stage without needing to run the whole pipeline.

`dvc param diff` will show you differences in values used in each parameter across different commits. Its close to how `git diff` is used. Learn more by reading [this](https://doc.dvc.org/command-reference/params/diff) article in official documentation.

