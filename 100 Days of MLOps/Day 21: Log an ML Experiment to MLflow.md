# Day 21: Log an ML Experiment to MLflow

## Problem

A data scientist at xFusionCorp Industries requires a training run to be recorded in MLflow in order to establish a baseline record on the tracking dashboard. The essential non-MLflow scaffolding has already been implemented in the script located at `/root/code/log_experiment.py`. Your objective is to complete the script by filling in the TODO blocks with the appropriate MLflow logging calls, ensuring that every aspect of the run is effectively captured by the MLflow tracking server.

1. The MLflow tracking server is already running on port `5000`. The MLflow UI button at the top of the lab can be opened to view the dashboard; the `Default` experiment is present on first load.
2. The script at `/root/code/log_experiment.py` prepares a `params` dictionary, fits a trivial sklearn model, and computes a pair of evaluation scores (`accuracy` and `f1`) from that model's predictions. Three blocks marked `# TODO` inside the `mlflow.start_run()` context are the only edits required.
3. Once the TODOs are completed and the script has been run, the end state must include:
    - A new run in the `Default` experiment.
    - Every hyperparameter in the `params` dict (`n_estimators=100`, `max_depth=5`, `random_state=42`) recorded as a run parameter.
    - Both computed scores (`accuracy`, `f1_score`) recorded as run metrics.
    - The sklearn model captured as an MLflow model artefact on the run.

> The result can be confirmed in the MLflow UI—once the run is opened, the Parameters, Metrics, and Artifacts panels each show the expected content.

## Solution

```python
import mlflow
import mlflow.sklearn

with mlflow.start_run():

    # TODO 1: log every entry in `params` as an MLflow parameter so that
    # n_estimators, max_depth, and random_state become searchable
    # parameters on this run.
    mlflow.log_params(params)

    # TODO 2: log `accuracy` and `f1` as MLflow metrics named
    # "accuracy" and "f1_score" respectively.
    mlflow.log_metrics({
        "accuracy": accuracy,
        "f1_score": f1
    })

    # TODO 3: log the trained `model` as an MLflow sklearn model
    # artefact on this run.
    mlflow.sklearn.log_model(model)

    print(f"accuracy={accuracy}, f1_score={f1}")
```

**Notes on syntax**
- To log parameters there are two ways. We can do single parameter logging through `mlflow.log_parameter("param_name", "value")` or multiple `mlflow.log_params({"param_1":"val_1", "param_2":"val_2"})`. [Documentation Link](https://mlflow.org/docs/latest/api_reference/python_api/mlflow.html#mlflow.log_params)
- When logging models based on the framework (_pytorch_, _sklearn_, etc.) the library can be changed (has to import correctly)

**Notes on MLFlow logging**

Mlflow logging is a tracking API used to track information regarding experiments run. It will be logged to the mlflow server setup in the previous challenge and can be viewed from the UI. This is beneficial for keeping track of experiments run including parameters, metrics, artifacts used/ produced in each run. For more information read the official docs for [tracking](https://mlflow.org/docs/latest/ml/tracking/), [manual logging](https://mlflow.org/docs/latest/ml/tracking/tracking-api/)

## Task Outputs

![mlflow tracking output](https://res.cloudinary.com/divjxx9rs/image/upload/v1791472369/mlflow_logging_e0up2b.png)