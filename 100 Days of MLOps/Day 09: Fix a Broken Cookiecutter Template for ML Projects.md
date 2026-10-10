# Day 9: Fix a Broken Cookiecutter Template for ML Projects

## Task Description

The xFusionCorp Industries Machine Learning platform team maintains a Cookiecutter template from which new ML projects are generated. A draft template is available at `/root/code/mlops-template/`, but it currently does not render properly. Correct the template and use it to generate a new project.


A Cookiecutter template exists at `/root/code/mlops-template/`. `cookiecutter` is installed system-wide, and the template is visible in the VS Code explorer. Run `cookiecutter /root/code/mlops-template/` to see how it currently fails to render.

The end state must satisfy every one of the following:

- The `cookiecutter.json` declares four variables:
    - `project_name` (default `my-ml-project`)
    - `author` (default `xFusionCorp`)
    - `python_version` (default `3.11`)
    - `ml_framework` with the choices `sklearn`, `pytorch`, and `tensorflow`
- The generated `requirements.txt` logic:
    - Contains `scikit-learn` when `ml_framework` is `sklearn`
    - Contains `torch` when `ml_framework` is `pytorch`
    - Contains `tensorflow` when `ml_framework` is `tensorflow`
- The generated `README.md` content:
    - Must reference both the `project_name` and the `author` from cookiecutter variables.
- The template directory structure `{{cookiecutter.project_name}}/` must contain:
    - Files: `README.md` and `requirements.txt`
    - Directories: `data/`, `models/`, `src/`, and `tests/`
- A project generated from the corrected template at `/root/code/churn-model/` (with p`roject_name=churn-model` and `ml_framework=sklearn`) contains a `requirements.txt` listing `scikit-learn` and a `README.md` that mentions `xFusionCorp`.

## Solution

**1. First let's add the `ml_frameworks` to the "cookiecutter.json"**

```json
{
    "project_name": "my-ml-project",
    "author": "xFusionCorp",
    "python_version": "3.11",
    "ml_framework": ["sklearn", "pytorch", "tensorflow"]
}
```

**2. Add `{% endif %}` and == in the requirements.txt**

```txt
{% if cookiecutter.ml_framework == 'sklearn' %}
scikit-learn
{% elif cookiecutter.ml_framework == 'pytorch' %}
torch
{% elif cookiecutter.ml_framework == 'tensorflow' %}
tensorflow
{% endif %}
```

**3. Make sure the `readme.md` has the correct author keyword**

```md
# {{cookiecutter.project_name}}

Created by {{ cookiecutter.author }}.
```

**4. Execution**

```bash
cookiecutter ./mlops-template
```

Project name should be "churn-model", ml_framework should be "scikit-learn". If it correctly works, no errors will produce and a correct README and a requirements.txt file will be generated.