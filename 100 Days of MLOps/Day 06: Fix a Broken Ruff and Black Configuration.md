# Day 6: Fix a Broken Ruff and Black Configuration

## Task Description

The xFusionCorp Industries ML team enforces code quality standards using `ruff` and `black` for every pull request. The current project located at `/root/code/fraud-detection/` is failing both tools. Apply the necessary modifications to ensure it passes the checks for both ruff and black.


The project at `/root/code/fraud-detection/` contains a `pyproject.toml` and sample sources under `src/`. ruff and black are already installed. From the project directory, run `ruff check src/` and b`lack --check src/` to see how they currently fail.

The end state must satisfy the following:

- ruff and black are both configured with a line length of 120.
- ruff lint rule selection includes `E`, `F`, `W`, and `I`.
- Running `ruff check src/` from the project directory exits with status 0.
- Running `black --check src/` from the project directory exits with status 0.


## Solution Configuration

```pyproject.toml
[project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 120
lint.select = ["E", "F", "W", "I"]

[tool.black]
line-length = 120
```

## Notes

- In ruff you can select an entire set of rules to be checked with their single letter alias instead of defining every rule (code). More info can be learned from the official [documentation](https://docs.astral.sh/ruff/rules/)
- Line length will tell ruff how long a line can be