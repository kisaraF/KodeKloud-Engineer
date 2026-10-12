# Day 25: Register, Version, and Manage Model Lifecycle

## Problem

The xFusionCorp Industries ML platform team requires the promotion of two trained candidates using the **MLflow Model Registry**. This will enable the operations team to monitor which model version is currently serving production traffic effectively. Both runs are already available in the `fraud-detection` experiment. Your task is to register both runs as versions of a new `fraud-detector` model, provide a model-level description, and assign the aliases `challenger` and `champion`, utilizing the MLflow UI.


1. The MLflow tracking server is already running on port `5000` and two runs are pre-populated in the `fraud-detection` experiment: a `baseline` run (`n_estimators=100`, `max_depth=5`, `f1_score=0.80`) and an `improved` run (`n_estimators=200`, `max_depth=10`, `f1_score=0.89`). Both runs can be opened via the **MLflow UI** button → `fraud-detection` experiment.

2. Using the MLflow UI, reach the end state below. The order (baseline first, improved second) matters because MLflow assigns version numbers sequentially within a registered model.

    - A registered model named `fraud-detector` exists in the Model Registry.
    The registered model carries a non-empty description that references the word `fraud` (any phrasing; for example `Fraud detection model for xFusionCorp transactions`).
    - **Version 1** of `fraud-detector` is the baseline run and carries the alias `challenger`.
    - **Version 2** of `fraud-detector` is the improved run and carries the alias `champion`.

> The result can be confirmed by opening **Model registry → fraud-detector** in the MLflow UI. Two versions are listed, the description is shown at the top of the model page, and the alias column (or the Aliases field on each version) indicates `challenger` on v1 and `champion` on v2.

## Solution

This can be done entirely with the MLflow UI only.

1. First log into MLflow and go to `agent versions` to see the experiment runs related models (should see improved and baseline models)
2. Then first go to `baseline` model and register it as a new model named `fraud-detector`.
3. Next to change its version, click on `model versions` so that it takes to the versions related page.
4. Since we are at version 1, change the alias to `champion` as requested.
5. To change the model description, you can click on `regisred models` from top (where it shows the path) and land to model page. Then go to `fraud-detector` model and change the description as requested. **Note** that this is a one time change for the model itself even though "version" and "agent versions" related descriptions are also available.
6. Next go to "agent versions" again and go to `improved` model and register it with 'fraud-detector`. Repeat the same thing for adding the version alias.
7. To check if the model versioning correct, go t o `Registered Models` and check for fraud-detector model's `Aliased version` section in the overview row