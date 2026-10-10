# Day 23: Search, Compare, and Triage MLflow Runs

## Task

A data scientist at xFusionCorp Industries has completed ten runs in the `fraud-detection` MLflow experiment. Your objective is to triage these runs within the MLflow UI. Begin by utilizing the search functionality to narrow down the results. Subsequently, compare the candidates side by side and identify the single best candidate to tag as shortlisted. Additionally, flag any runs that are clearly underperforming for removal.

1. The MLflow tracking server is already running on port `5000`, and the `fraud-detection` experiment has been pre-populated with ten runs (each carrying `n_estimators`, `max_depth`, `accuracy`, and `f1_score`). The runs can be viewed via the **MLflow UI** button → `fraud-detection` experiment.

2. Work through the MLflow UI's **Search** bar (to filter runs with the `metrics.*` query syntax) and its **Compare** view (to inspect the contenders side by side) to reach the triage end state below. The end state is what is tested—the path taken through the UI is not.
- **Shortlist the best candidate**. Among all runs where `metrics.f1_score > 0.85`, the single run with the highest `f1_score` must carry a run-level tag: key `review-status`, value `shortlisted`.
- **Reject the under-performers.** Every run where `metrics.f1_score < 0.75` must carry a run-level tag: key `review-status`, value `rejected`.

3. The other runs (those in the 0.75 ≤ f1 ≤ 0.85 band, and the second-best shortlisting candidate) must carry no `review-status` tag at all.

## Solution

1. Go to mlflow ui, `fraud-detection` experiment and "Evaluation runs".
2. Next in the search bar type `metrics.f1_score > 0.85` find the best scores and filter them from the f1 score value
3. Add a tag to the highest f1 score associated run `review-status: shortlisted`
4. Then filter for undeperformed runs `metrics.f1_score < 0.75`
5. Add a tag as `review-status: rejected`
