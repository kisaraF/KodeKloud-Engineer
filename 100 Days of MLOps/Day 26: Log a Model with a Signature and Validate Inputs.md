# Day 26: Log a Model with a Signature and Validate Inputs

## Problem

The xFusionCorp Industries deployment team requires that every model promoted to serving includes an input schema to ensure that malformed requests are rejected at the boundary, preventing the occurrence of silent errors. A logging script along with the necessary data and model has been pre-staged. Your task is to attach an MLflow model `signature` and an `input example` during the model logging process.

The MLflow tracking server is already running on port `5000`. The **MLflow UI** button at the top of the lab can be opened to view the dashboard.
The script `/root/code/log_with_signature.py` builds a small synthetic two-column training frame (`amount`, `num_txn`), fits a model, and computes its predictions — all already written. Attaching a signature is inherently a code step (there is no UI control for it), so the work is in the script. Two `# TODO` blocks remain inside the `mlflow.start_run()` context:

- `TODO 1`: Infer the model signature from the training inputs `X` and the model's predictions `preds`, and bind it to signature.
- `TODO 2`: Log the sklearn model under the artefact name model with the inferred signature and an input_example (pass `X`), so the logged model carries an enforced input schema.

Once both TODOs are completed and the script has been run, the end state must include:
- An experiment named `fraud-signature` with at least one run.
- The logged model carries a signature whose input schema is exactly the columns `amount` and `num_txn`.

> The schema is enforced: loading the model and predicting with a frame that omits a required column (e.g. num_txn) is rejected, while a correctly-shaped frame predicts successfully.

## Solution

**1. Complete the mlflow logging as below**

```python
with mlflow.start_run():
    # TODO 1: infer the model signature from the training inputs `X` and
    # the model's predictions `preds`, and bind it to `signature`.
    # Use mlflow.models.infer_signature(X, preds).
    signature = infer_signature(X, preds)

    # TODO 2: log the sklearn `model` under the artefact name "model"
    # WITH the inferred `signature` and an `input_example` (pass `X`) so
    # the logged model carries an enforced input schema. Use
    # mlflow.sklearn.log_model(model, name="model", signature=...,
    # input_example=...).
    mlflow.sklearn.log_model(model, name='model',input_example=X, signature=signature)

print("model logged to the fraud-signature experiment")
```

_MLflow sklearn model logging [guide](https://mlflow.org/docs/latest/api_reference/python_api/mlflow.sklearn.html#mlflow.sklearn.log_model)_

**2. Model schema and signature can be viewed as below later

![Model artifact schema and signature](https://res.cloudinary.com/divjxx9rs/image/upload/v1791861767/Model_Artifact_Signature_Schema_klpvnm.png)

## Notes

`Model signature` is like a contract/ structure how the input, output, parameters should look like and `input example` is a concrete example of the data to be fetched into the model so that it will be consistent, documented and easy to validate. To read further, refer to this [complete documentation](https://mlflow.org/docs/latest/ml/model/signatures/).