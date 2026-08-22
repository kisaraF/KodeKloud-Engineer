# Day 7: Test and Package the Fraud-Detection Module

## Task Description

The xFusionCorp Industries deployment team requires the fraud-detection module to be validated through unit tests and to be packaged as an installable Python distribution. The module's source code and a draft `pyproject.toml` file can be found at `/root/code/fraud-detection/`. Your task is to create unit tests for the module, rectify the packaging configuration, and build a compliant wheel.


The project at `/root/code/fraud-detection/` contains the module source under `src/fraud_detection/` — a `predict()` function that flags a transaction as fraud when its amount (the first feature value) exceeds `100`. The source is complete; you do not need to modify it. `pytest` and `build` are already installed. Use `python3` rather than `python`.

The end state must satisfy the following:

- **Unit tests**: `tests/test_predict.py` contains at least two tests that import `predict` from `fraud_detection` and assert on its output — one fraudulent row (amount > 100, expect `1`) and one legitimate row (amount <= 100, expect `0`); `pytest` run from the project directory passes.
- **Packaging configuration**: the corrected `pyproject.toml` satisfies every one of the following:
    - a `[build-system]` section with `requires = ["setuptools>=61.0", "wheel"]` and `build-backend = "setuptools.build_meta"`;
    - `name` is `fraud_detection`;
    - `version` is `0.1.0`;
    - `requires-python` is `>=3.10`;
    - `dependencies` is `["scikit-learn", "pandas", "numpy"]`;
    - pytest can import the package from `src/` — declare `[tool.pytest.ini_options]` with `pythonpath = ["src"]`.
Built artifact: building the package produces a wheel named `fraud_detection-0.1.0-*.whl` under `dist/`.


## Solution

### PyTest Tests

```python
from fraud_detection.predict import predict
# import like this is possible due to configuration made in the pyproject for pytest specifically

def test_fraud_1():
    data = [[10,20], [301,40]]
    ev = [0,1]

    result = predict(data)

    assert result == ev


def test_fraud_2():
    data = [[101,20], [30,40]]
    ev = [1,0]

    result = predict(data)

    assert result == ev
```

### `pyproject.toml` configuration

```toml
[project]
name = "fraud_detection"
version = "0.1.0"
description = "Fraud detection model for xFusionCorp Industries"
requires-python = ">=3.10"
dependencies = [
    "scikit-learn",
    "pandas",
    "numpy"
]

[tool.setuptools.packages.find]
where = ["src"]

[tool.pytest.ini_options]
pythonpath = ["src"]

[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"
```

- `[tool.setuptools.packages.find]` is read by the build tool to find where the packages are. You can make it explicit by telling where to look, what not to look with options like `include = ` and `exclude = `. In other terms ***"how do I find code to package into a wheel"***. This config line defines from tool to tool, therefore checking the docs is recommended. Some tools like uv doesn't need this as it can auto-discover the packages.
- Since package code is sitting in the `src/` and it's not accessible because each time when Python do a module import it checks for path's files through `sys.path`. And the config `[tool.pytest.ini_options]` lets pytest know where the package code lives. ells pytest: ***"before collecting and running tests, add these directories to sys.path"***
- **What's a build backend?**: `Build system` is what's responsible for turning source code into installable and distributable packages. `Build front` end is usually what we use to resolve dependencies to ochestrate the build (pip, uv, poetry, etc.). `Build backend` is what can be used to turn the source code into a wheel file. So this wheel file can be installed using build frontend like uv, pip, etc.
- For more details information regarding Python packaging refer to below documentation,
    - [Packaging Python Projects](https://packaging.python.org/en/latest/tutorials/packaging-projects/)
    - [Distributing Packages Using setuptools](https://packaging.python.org/en/latest/guides/distributing-packages-using-setuptools/)
    - [Official Python Packaging Guides](https://packaging.python.org/en/latest/guides/)

### Build artifact

To build a wheel file from the package, run the following command. **Important:** First confirm if the pytests are working properly. 

```bash
python3 -m build
```

Wheel files are **pre-built, ready-to-install** distribution.

Wheel files are used in following situations:
1. **Speed matters**: Some projects where there are a lot of dependencies, getting them installing in a matter of seconds is important. 
2. **Security and no toolchain needed**: In production machines, CI runners, docker images, [toolchain](https://en.wikipedia.org/wiki/Toolchain) is often needed to build and develop software. So usually when used a wheel file this requirmeent is not needed. Some runners could have strict policies with no access to internet to download dependencies. In such cases, this is a better alternative.
3. **A deterministic artifact**: It's proof the project is a valid installable package which can be handoff to a registry, teammate, etc. 