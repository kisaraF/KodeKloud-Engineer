# Day 17: Run and Compare DVC Experiments

## Task Description

The xFusionCorp Industries MLOps team needs every model training run to be reproducible, automatically tracked, and easy to compare so a chosen configuration can be promoted into version control. The fraud-detection pipeline is parameterized by `max_depth`, currently set shallow enough to underfit. Using DVC experiments, run three tracked experiments over different `max_depth` values, compare their recorded f1_score on the held-out test set, and promote the best-scoring run so its parameters, metrics, and model become the tracked workspace state.


A project exists at `/root/code/fraud-detection/` with a parameterised DVC pipeline already in place. params.yaml declares `n_estimators: 100` and `max_depth: 4`, and the baseline pipeline has been run once. `src/models/train.py` reads both parameters, trains the model, and evaluates it on the held-out test set, writing the real accuracy and `f1_score` to metrics.json. Do not modify the Python files.

**Acceptance criteria:**

- Three DVC experiments have been run, each with a different value for `max_depth` across a reasonable range (for example `2`, `6`, and `12`); each experiment retrains the model and produces a fresh `metrics.json`.
- The experiment with the highest `f1_score` is applied to the workspace, so its `max_depth`, `metrics.json`, and `models/model.pkl` become the tracked state.

> The DVC extension's EXPERIMENTS view (open the DVC panel from the Activity Bar) lists every experiment alongside its parameters and metrics, which is a convenient way to compare runs at a glance.

## Solution

DVC experiments are great to run multiple different experiment hyper parameter tuning and select the best one that fits the model. Often we would do this manually and note the hyper parameter metrics values manually to track. But the below work line will run N number of experiments without modifying thr workspace and let use apply only the required experiment values. This saves us from having to deal with local workspace being messy when experimenting.

**1. First build a queue with experiments to be run**

```bash
dvc exp run --queue --set-param "max_depth=2,6,12"
```

This will create experiments to be run, assign an ID and a nick name to each and keep them queued. `dvc queue status` will show the run status of each queue

**2. Run the experiments**

```bash
dvc queue start
```

This will run all queues sequentially. To run them parallel `-j` flag can be used as `dvc queue start -j 4` which tells how many parallel jobs are needed (this depends on the machine resources)

**3. See experiment results**

```bash
dvc exp show --only-changed
```

This will show each experiment's parameter and metrics change against the HEAD values in a table

```bash
dvc exp show --json
```

This will print out the experiments, dvc tracking metadata, etc. in-detailed

**4. Applying an experiment to workspace**

```bash
dvc exp apply <name or id>
```

By doing this, the workspace will apply the values of the experiment, lock file will be updated, and everything will be updated as if you have done a `dvc repro` command. 

## Further reading

1. [Running experiments](https://doc.dvc.org/start/experiments/experiment-pipelines)
2. [dvc queues](https://doc.dvc.org/command-reference/queue/start)