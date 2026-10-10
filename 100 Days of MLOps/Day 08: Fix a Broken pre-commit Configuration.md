# Day 8: Fix a Broken pre-commit Configuration

## Task Description 

The xFusionCorp Industries ML team promotes code quality for every commit by utilizing `pre-commit`. A draft `.pre-commit-config.yaml` file is located in the git repository at `/root/code/fraud-detection/`. However, this configuration does not align with the team's standards, resulting in a failure when executing `pre-commit run --all-files`. Revise the configuration to ensure compliance with the team's requirements.

A git repository already exists at `/root/code/fraud-detection/` with `.pre-commit-config.yaml` and `process.py` already tracked. `pre-commit` is installed system-wide. From the project directory, run `pre-commit run --all-files` to see how the current configuration fails.

The end state must satisfy the following:

- the configuration declares these five hooks so that `pre-commit run --all-files` executes every one of them:
    - `trailing-whitespace`, `end-of-file-fixer`, and `check-yaml` – All three sourced from the `pre-commit/pre-commit-hooks` repository, pinned to a current release;
    - `ruff` – Sourced from the `astral-sh/ruff-pre-commit` repository, pinned to a current release;
    - `black` – Sourced from the `psf/black-pre-commit-mirror` repository, pinned to a current release;
- every repository entry in the configuration includes a `rev:` field;
- the hooks are registered with git and run cleanly against the tracked files.

## Solution

**Change the configuration to the following**

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0 # version should be the current release
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml # name should be correct here. original config had it wrong

  - repo: https://github.com/astral-sh/ruff-pre-commit # this link should be current
    rev: v0.16.2 # version should be the current release
    hooks:
      - id: ruff # this id should be correct as this. It's legacy but needed for this exercise. But ruff-check seems more modern

  - repo: https://github.com/psf/black-pre-commit-mirror
    rev: 26.5.1 # version should be the current release and must have a rev
    hooks:
      - id: black
```

- Running the command `pre-commit autoupdate` will pin all the hooks into the current version by synicing them through the repo link.
- Running the command `pre-commit run --all-files` will check the commit hook without needing the git commit command at all.

## Notes

- Pre-commit is tied to current project. When it's first created by running a command (run or git commit), it creates a cache inside ***`.git/hooks/pre-commit`***
- Pre-commit manages its own isolated virtual environment at ***`~/.cache/pre-commit`*** for each repo. Dependencies are installed in that cache too.
- You can add your own commits as well. See [this](https://medium.com/data-science/custom-pre-commit-hooks-for-safer-code-changes-d8b8aa1b2ebb) guide for more info ___(subject to change upon testing)___
- If you already have a tool configuration (ruff, black, sqlfluff) in the project, the pre-hook will adhere to it as the commands are based on what's in the project.
- You can easily install pre-commit though virtual environment like uv. Once the command `pre-hook install` is run, there will be a directory built in the `.git/hooks` which will help `git commit` to run pre-hook checks without the virtual environment to be in an active state.

### References

- [Official pre-commit documentation](https://pre-commit.com/)
- [Official Doc supported hooks](https://pre-commit.com/hooks.html)
- [All available pre-commit out-of-box hooks](https://github.com/pre-commit/pre-commit-hooks/blob/main/.pre-commit-hooks.yaml)
- [Guide to using pre-commit hooks](https://dev.to/vaiolabs_io/amazing-pre-commit-and-how-to-use-it-5enb)