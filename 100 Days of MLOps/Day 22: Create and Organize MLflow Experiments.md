# Day 22: Create and Organize MLflow Experiments


## Problem

You are tasked with onboarding two new ML projects for the xFusionCorp Industries ML platform team. It is essential that each project is organized under its own MLflow experiment, rather than sharing the `Default` experiment. Proceed to register both experiments using the MLflow UI, ensuring that they are correctly tagged with the owning team.


1. The MLflow tracking server is already running on port 5000. The MLflow UI button at the top of the lab can be opened to view the dashboard. One seeded experiment (legacy-models) is listed alongside the platform-created Default—both act as reference material and must not be modified.
2. Using the MLflow UI, register two new experiments with the experiment-level metadata below. The task is complete when both records satisfy every bullet.
    - `fraud-detection`
        - Experiment-level description is a non-empty string describing the project (any phrasing).
        - Experiment-level tag: key `team`, value `ml-platform`.
    - `churn-prediction`
        - Experiment-level tag: key `team`, value `analytics`.

> The result can be confirmed in the MLflow UI: both new experiments appear in the left-hand list, with the description and tags visible on each experiment's page.

## Solution

This task can be done via MLFlow UI entirely. First open the MLFLOW UI. Then go to experiments. Then you can create a new experiment through the button on top-right coroner. Make sure give the name of the experiment and add the tag's key-value pair for each experiment. To add a description, open a experiment and in the top-right coroner there's three dots which can be used to add the description. Once the description is added, it can be viewed like below.

![created mlflow experiments](https://res.cloudinary.com/divjxx9rs/image/upload/v1791529591/Custom_mlflow_experiments_gxmg3l.png)