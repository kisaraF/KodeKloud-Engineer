# Day 20: Start the MLflow Tracking Server

## Problem

The xFusionCorp Industries ML team is in the process of adopting MLflow for their experiment tracking. Your task is to set up a local MLflow tracking server on the ML pipeline workstation, enabling the team to log experiments from their training code.

MLflow is pre-installed on the controlplane. Launch the tracking server in the background and choose the flags that satisfy every end-state requirement below.

1. The server listens on port `5000` and is reachable on all network interfaces, not only `localhost`.
2. The backend store is a SQLite database at `/root/code/mlflow-backend/mlflow.db.` Create any parent directory first — MLflow aborts at startup if the backend directory is missing.
3. The artifact root is `/root/code/mlflow-artifacts/.`
4. The MLflow UI button at the top of the lab routes through the lab proxy, which reaches the server with a non-`localhost` host header and a different origin. Launch the server so it accepts any host header and any origin; otherwise the button returns a 403 or CORS error.
5. The server process persists in the background so it survives terminal closure.

> Once the server is running, the `Default` experiment can be viewed from the MLflow UI button. The experiment is empty.

## Solution

**1. First make the required directories**

```bash
mkdir mlflow-backend mlflow-artifacts
```

**2. Check the mlflow server**

```bash
mlflow server --port 5000 --backend-store-uri sqlite:////root/code/mlflow-backend/mlflow.db --default-artifact-root /root/code/mlflow-artifacts --host 0.0.0.0 --cors-allowed-origins '*' --allowed-hosts '*'
```

- `backend--store` is a database that writes metadata like experiment IDs, Model related info, etc. By default it is a SQLite database but it can be changed based on preference.
- `artifact-root` is the local directory/ uri where the artifacts are being stored
- By adding `host` as `0.0.0.0`, we allow any _ to 

**3. Persiste server process in background**

By using `nohup` command in native Linux, this can be achieved. A `nohup.out` file is written with stdout.

```bash
nohup mlflow server --port 5000 --backend-store-uri sqlite:////root/code/mlflow-backend/mlflow.db --default-artifact-root /root/code/mlflow-artifacts --host 0.0.0.0 --cors-allowed-origins '*' --allowed-hosts '*'
```

_to be further updated with MLFlow use cases and purpose_