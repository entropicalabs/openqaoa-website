# Installing

## Install latest stable through pip

You can install the latest version of OpenQAOA [directly from PyPI](https://pypi.org/project/openqaoa/). Creating a python `virtual environment` for this project is recommended. (for instance, instructions on how to create a virtual environment using conda can be found [here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands). Make sure to use a python version between **python 3.8** and **python 3.10**. 

Once the virtual environment is activated, then simply pip install openqaoa with the following command

```bash
pip install openqaoa
```

## Install dev and experimental features

Alternatively, you can install it manually directly from the GitHub. This may be relevant if you plan to contribute to the development of OpenQAOA or if you want to try out the latest features parked on the `dev` branch on our github repository.

To install from github first

1. Clone the git repository:

```bash
git clone https://github.com/entropicalabs/openqaoa.git
```

!!! note
    If you want to test some experimental features, after cloning the repository feel free to checkout the dev branch. It is simple, just type `git checkout dev` :)

2. Creating a python `virtual environment` for this project is recommended. (for instance, using conda). Instructions on how to create a virtual environment can be found [here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands). Make sure to use a python version between **python 3.8** and **python 3.10**.

3. After cloning the repository `cd openqaoa` and pip install the package. Use the following command for a vanilla install with the `scipy` optimizers:

```bash
pip install .
```
If you are interested in running the tests or the docs you can do so by using the installment modifiers `[docs]` and `[tests]`. For example,

```bash
pip install .[tests]
```

Should you face any issues during the installation, please drop us an email at [openqaoa@entropicalabs.com](openqaoa@entropicalabs.com) or [open an issue](https://github.com/entropicalabs/openqaoa/issues)!