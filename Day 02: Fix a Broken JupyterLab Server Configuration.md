# Day 2: Fix a Broken JupyterLab Server Configuration

## Task Description
- Fix the jupyterlab configuration file with the following changes:
    - Binding IP should be `0.0.0.0`
    - Port should be `8888`
    - Root directory should be `root/notebooks`

## Solution

**1. Activate the virtual environment**

```bash
cd /root/code
source ml-env/bin/activate
```

**2. Run the configuration file to start the server to check errors**

```bash
jupyter lab --config /root/code/jupyter_lab_config.py
```

**3. Make the above changes to the configuration file**

```python
c.ServerApp.root_dir = '/root/notebooks/'
c.ServerApp.port = 8888
c.ServerApp.ip = '0.0.0.0'
```

**4. Make sure the `notebook` directory exist in the disk**

```bash
mkdir /root/notebooks
```

**5. Start the server and launch Jupyter UI**

```bash
jupyter lab --config root/code/jupyter_lab_config.py --allow-root
```

## Notes

Jupyter Lab is an evaluation of mix of Jupyter notebooks, code editor, terminal, etc. used for data science and ML work. It can be set up with a virtual environment as above task pretty easily. We can use either uv or native Python3 virtual environment to achieve it.

By using the configuration file `jupyter_lab_config.py` we can have our settings set rather than manually setting them everytime upon launching:
- Root directory of notebooks
- Custom ports and ip bindings to avoid conflicts and access through remote machines
- Set passwords, tokens
- Adjust resource limits

Configuration file enables automation, shareability, security, consistency.

To create a config file, we can do 

```bash
jupyter lab --generate-config
```

This will create the configuration at `/home/user/.jupyter/jupyter_lab_configuration.py`. And this configuration will have a lot of setting but commented out. Setting them up needs uncommenting and configuring. Then you can start the server

```bash
jupyter lab --config /path/to/specific/config/.jupyter_lab_config.py
```

You can have a global config in the default path. And if you want a project specific confiuration that can be done as below


You can even set up the configuration exclusively to the project directory like below (make sure venv is activated always)

```bash
export JUPYTER_CONFIG_DIR=/home/user/data-science/
jupyter lab --generate-config
```

*Opinion: This is much better than using Anaconda environment which is heavy*