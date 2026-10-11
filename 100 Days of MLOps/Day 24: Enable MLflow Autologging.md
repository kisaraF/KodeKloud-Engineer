# Day 24: Enable MLflow Autologging

## Problem

The xFusionCorp Industries ML team intends to enhance their training scripts by replacing the manual `log_param` and `log_metric` boilerplate code with MLflow's autologging feature. This integration will ensure that every training run automatically captures its constructor parameters, training metrics, and model artifacts. A training scaffold has been prepared and is located at `/root/code/autolog_experiment.py`. This scaffold configures MLflow, fits a small synthetic sklearn model, and prints a confirmation message. There are two `# TODO` blocks that require completion. Your task is to successfully fill in these blocks to achieve the desired end state as specified.

1. The MLflow tracking server is already running on port `5000`. The **MLflow UI** button at the top of the lab can be opened to view the dashboard; only the `Default` experiment is present on first load.

2. The scaffold at `/root/code/autolog_experiment.py` carries two `# TODO` blocks (each a one-line addition). Once they are completed and the script has been run, the following end state holds:

    - An experiment named `autolog-demo` exists on the MLflow server.
    - At least one run exists in the a`utolog-demo` experiment.
    - The run's **Parameters** panel lists every sklearn constructor parameter that the `LogisticRegression` in the scaffold implicitly carries (for example `C`, `max_iter`, `solver`, `tol`, `penalty`) – Not only the three explicit keyword arguments the scaffold passes.
    - The **Artifacts** panel on the run contains a model directory with an `MLmodel` descriptor and a pickled estimator.

> No real dataset is loaded by the scaffold—the training step is a deterministic toy that gives MLflow a `.fit()` call to observe. The focus of the lab is autolog configuration, not model quality.

## Solution


```python
"""
MLflow autologging — two TODO blocks activate MLflow's automatic
capture of parameters, metrics, and the trained model for the
`model.fit(...)` call below.

The dataset and the model here are synthetic. A LogisticRegression
fitted on a deterministic four-row XOR-like array stands in for a
real training step so that autologging has a valid sklearn fit()
call to instrument. No real ML workflow takes place; the focus of
the lab is autolog configuration, not model quality.

Both TODO blocks must be completed BEFORE `model.fit(...)` runs —
autolog hooks sklearn at call time, and the active experiment
scopes where the autologged run lands.
"""
import numpy as np
import mlflow
import mlflow.sklearn
from sklearn.linear_model import LogisticRegression

mlflow.set_tracking_uri("http://localhost:5000")


# TODO 1: enable autologging for the sklearn flavour so that the
# subsequent model.fit(...) call records parameters, metrics, and
# the trained model on the active experiment automatically.

mlflow.sklearn.autolog()

# TODO 2: set the active experiment to "autolog-demo" so the
# autologged run lands in that experiment rather than the Default one.

mlflow.set_experiment("autolog-demo")

# Synthetic four-row XOR-like array. Not a real ML dataset — just
# a deterministic toy to give sklearn.fit() something to execute.
X = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])
y = np.array([0, 0, 1, 1])

model = LogisticRegression(C=1.0, max_iter=100, random_state=42)
model.fit(X, y)

print("Autolog run complete — check the MLflow UI")
```