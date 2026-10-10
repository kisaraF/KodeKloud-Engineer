# Day 1: Create a Python Virtual Environment for ML

## Task Description:
- Create a Python virtual environment in the directory /root/code as `ml-env`
- Install the packages `numpy`, `pandas`, `scikit-learn`, `matplotlib`
- Add a requirements file of the installed packages to /root/code/ directory

## Solution

**1. Create the virtual environment**

```bash
cd /root/code
python3 -m venv ml-env
source ./ml-env/bin/activate
```

**2. Install the packages**

```bash
pip3 install numpy pandas scikit-learn matplotlib
```

**3. Add a requirements file**

```bash
pip3 freeze > requirements.txt
```