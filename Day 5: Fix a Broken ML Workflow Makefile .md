# Day 5: Fix a Broken ML Workflow Makefile

## Task Description

The xFusionCorp Industries Machine Learning team utilizes a `Makefile` to streamline essential tasks such as data processing, training, testing, and cleanup. A preliminary Makefile can be found at `/root/code/fraud-detection/Makefile`, but the execution of make all does not yield successful completion. Ensure that the Makefile is aligned with the team's standards.


A Makefile lives in /`root/code/fraud-detection/`. Run `make all` from the project directory to see how it currently fails.

The end state must satisfy the following:

- the Makefile declares these six targets and behaviour:
	- `setup` – Creates a virtual environment at `mlops-venv/` and installs dependencies from `requirements.txt`;
	- `data` – Runs `python3 src/data/process_data.py`;
	- `train` – Runs `python3 src/models/train.py`;
	- `test` – Runs `pytest tests/`;
	- `clean` – Recursively removes every `__pycache__` directory, removes `.pytest_cache`, and clears the contents of `models/`;
	- `all` – Runs `setup`, `data`, `train`, and `test` in that order;
- all six target names are declared as `.PHONY` so that Make never confuses them with files of the same name; `make all` completes without error.
- Makefile recipes must be indented with a real tab character, not spaces. Make rejects any recipe that is not tab-indented.

## Solution

**1. Run the `make all` command**

It outputs ___`Makefile:7: *** missing separator. Stop.`___

**2. Identify Errors in the file**

```bash
cat Makefile -A
```
Running the above command will show what's wrong in the formatting. A Makefile must have a tab instead of spaces for the recipe of the target. Similar output as below will be expected.

![makefile output](https://res.cloudinary.com/divjxx9rs/image/upload/v1786257142/makefile_output_t9szkw.png)

Then make the changes of the formatting

**3. Produce a clean Makefile with the following checks passed**

- Formatting corrected
- PHONY targets added
- all target declared with `data`, `train`, `test` targets
- recuresive removal of `__pycache__` in "clean" target

```makefile
# fraud-detection Makefile
.PHONY: setup data train test clean all

setup:
	python3 -m venv mlops-venv && mlops-venv/bin/pip install -r requirements.txt

data:
	python3 src/data/process_data.py

train:
	python3 src/models/train.py

test:
	pytest tests/

clean:
	find . -type d -name __pycache__ -exec rm -rf {} +\n\trm -rf .pytest_cache\n\trm -rf models/*

all: setup data train test
```

## Notes

- Makefile is a laguage agnostic build automation, file-centric tool which builds based on changes made to files. 
- The name you give to a command is known as the `target`
- The command of a target is known as `recipe`
- `.PHONY` is used to declare targets as targets by saying "names declared here are not files but alias for a command". If it's not declared, and if someone runs a command like `make all` and if there's a folder named `all` it will do nothing

This is useful especially in following scenarios:
- Bootstrapping the engineering environment: Tie sophisticated commands or multiple long build commands into one keyword like `make build`
- CI/CD pipelines: Instead of writing 10 steps, a makefile will replace all that in one command. Unless you have no special conditions to add in each step or dynamic context objects to refer to (except for runner deps installation and cache/ checkout code actions)