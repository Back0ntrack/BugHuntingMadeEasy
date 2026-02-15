# Python Virtual Environment

A Virtual Environment is a Python environment, that is an isolated working copy of Python that allows you to work on a specific project without affecting other projects So basically it is a tool that enables multiple side-by-side installations of Python, one for each project.

## Creating a Python Virutal environment in Kali

* Install pip if not installed

```
sudo apt install python-pip
```

* Install virtualenv

```
pip install virtualenv
```

* Create a virtual environment

```
virtualenv myenv [default to python3]
```

* Activate the virtual environment

```
source <virtual_env_name>/bin/activate
```

* To deactivate the virtual environment

```
deactivate [in the virtual environment]
```

<figure><img src="../../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

## Python 2.7 environment setup \[Most Useful]

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Reinstall setuptools and pip for Python 2.7
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -o get-pip.py
sudo python2.7 get-pip.py
python2.7 -m pip install --upgrade setuptools wheel
python2.7 -m virtualenv old_python
source old_python/bin/activate
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

> When creating a virtual environment in Kali Linux, a folder with the same name is generated in the current directory. It's recommended to create all virtual environments inside a dedicated `virtual_env` folder for better organization and easy access.

## Pipx

`pipx` is a tool to install and run Python CLI tools in isolated environment, so they don't pollute your global or project environment.&#x20;

**Features:**&#x20;

* Each tool you install gets its **own virtual environment**.
* Makes command-line tools available globally, cleanly.

Generally tools are installed in a dedicated python environment and can be accessed in `~/.local/share/pipx/venvs` whereas executables are located in `~/.local/bin`. `pipx ensurepath` ensures that the pipx bin directory is correctly added to your PATH environment variable. &#x20;

<figure><img src="../../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>
