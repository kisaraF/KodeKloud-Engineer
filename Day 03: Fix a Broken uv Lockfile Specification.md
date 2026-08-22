# Day 3: Fix a Broken uv Lockfile Specification

## Task Description

The xFusionCorp Industries ML team utilizes uv and lockfiles to maintain consistent Python dependencies across different machines. A teammate has submitted a `requirements.in` specification that does not adhere to the team's standards. Correct the specification and compile it into a pinned lockfile.


A high-level dependency specification exists at `/root/code/fraud-detection/requirements.in`, but it does not match the team's standards. uv is already installed.

The end state must satisfy the following:

- The corrected `requirements.in` lists exactly these four top-level packages: `scikit-learn`, `mlflow`, `pandas`, and `numpy`, with any version constraint being one uv can satisfy against PyPI (bare package names are fine — uv pins exact versions when it compiles the lockfile);
- A pinned lockfile `requirements.txt` is compiled from the corrected specification, pinning each of the four top-level packages to an exact version using == and including the transitive dependencies that uv resolved.


```
========= requirements.in =========
sklearn
mlflow>=100.0
numpy
```

## Solution

**1. First manually correct the packages in the lock file**

```
========= requirements.in =========
scikit-learn
mlflow
pandas
numpy
```

**2. Now initiate uv in the folder**

```bash
cd fraud-detection
uv init
```

**3. Now install the lock file** _(This is totally optional. Compilation of requirement.txt with dependencies will automatically happen)_

```bash
uv add -r requirements.in
```

**4. Now create a `requirements.txt` file***

```bash
uv pip compile requirements.in -o requirements.txt
```


## Notes

- `requirements.in` file is a lose typed (write by hand) file where you define the dependencies you need without a strict version (unless you don't have). It expresses intent: _"my project needs these things,"_ not _"here's exactly what to install."_
- `requirements.txt` is the file which is fully resolved with correct versions pinned, that will be used in production to install dependencies
- `pyproject.toml` is the modern way of managing dependencies. When you run `uv add ...` the packages with versions installed are added here, replacing `requirements.in`
- `uv.lock` is the uv's own format for adding dependency graph. This gets updated everytime adding, removing packages. It is mentioned that, this is more effective than pip.

**Why use requirements... files in a uv environment**

1. As a production artifact `requirements.txt` is needed. UV act as the development tool for fast downloads, etc. But often most of the production environments do not have uv. Therefore, a requirements.txt is needed
2. A lot of platforms require you the `requirements.txt` style to implement
3. Universal- Downstream users of your project, might not use uv. But if they use Python, pip based requirements is something they definetely can work with
4. CI/CD tools, scanners require the requirements.txt way as it is what the whole Python ecosystem understand (as uv.lock is newer)
