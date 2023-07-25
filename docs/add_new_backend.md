# Adding a new Backend Plug-in in OpenQAOA

Since OpenQAOA is designed with modularity and multi-backend support in mind a user can easily add a new backend plugin in OpenQAOA.
OpenQAOA distinguishes between parts of the QAOA workflow that are a backend agnositc and backend dependent. The backend agnostic parts that can be abstracted away uniformly across all devices and cloud providers form the core functionality of the package, and are therefore, placed in `openqaoa-core`. The parts that are specific to each backend device or cloud provider are placed in their respective plugins. Therefore, to run a QAOA workflow on Amazon AWS quantum computers, one needs `openqaoa-core` for the backend agnostic components and `openqaoa-braket` to communicate with the devices on AWS Braket. 

In similar spirit, one can add new backends that add a new hardware provider to the existing list of openqaoa plugins. This tutorial provides a guide on how to begin adding a new backend plugin. For the rest of this tutorial, we assume that the new backend will be called `xyz`

## Create the new plugin folder

- Create a new folder `/src/openqaoa-xyz`

This folder will contain all the necessary code to support the new `XYZ` plugin. 
Within this folder will be the following files containing instructions to convert the plugin into an installable package:
- LICENSE: Since OpenQAOA is licensed using MIT License, the individual plugins will follow the same license as well.
- MANIFEST.in: This file will include the requirements file to install package dependencies for `openqaoa-xyz`
- pyproject.toml: This file should contain the following instructions
	```Python
	[build-system]
	requires = ["setuptools>=61.0"]
	build-backend = "setuptools.build_meta"
	```
- README.md: A readme file to describe the plugin
- requirements.txt: A textfile describing all the necessary python dependencies for the plugin
- setup.py: Setup instructions to install the package as a python module
- tests/ : A tests folder containing all backend specific unit-tests for the new plugin

All the code supporting this new plugin will go inside a `src/openqaoa-xyz/openqaoa_xyz`. The naming conventions of all files and folders are crucial for the proper functioning of the plugin.

### `openqaoa_xyz`
The folder contains the following:
- `__init__.py`: 
- `_version.py`: This file should set the same version number of the plugin as `openqaoa-core`
- backend_config.py: This file should contain instructions on how the plugin device object is paired to the QAOABackend object. 

Finally, the code implementation of the device and backend lives inside the `src/openqaoa-xyz/openqaoa_xyz/backends` folder. This folder contains three different kinds of files:

- `gates_xyz.py`: This file provides a mapping between the gates in the OpenQAOA intermediate representation and the gates of the backend, and an implementation of the `apply_gate` methods that are used by `openqaoa-core` to construct the QAOA circuit. All these functions are implemented as methods of the `XYZGateApplicator` class.

- `devices.py`: Implement the `DeviceXYZ` object here. The user must pay attention towards the requirements for the specific cloud provider and their methods of authentication. These must be built into the device object. Moreover, this object must inherit from the `DeviceBase` object defined in `openqaoa-core`

- `qaoa_xyz.py`: The name of this file can be chosen at will. The file should contain the QAOA Backend implementation for the `XYZ` hardware provider. It must contain the basic methods expected from a QAOA Backend class, for instance, `qaoa_circuit`, `get_counts` and so on. More formally, this class is responsible for converting the QAOA circuit from the OpenQAOA intermediate representation into the respective language supported by the `XYZ` hardware provider. Some template classes are defined in `openqaoa-core/openqaoa/backends/basebackends.py` that can be used as parent classes for this class. For instance, a QAOA Backend class implemeting a QPU that requires authentication, supports parametric circuit construction must inherit from `QAOABaseBackendShotBased`, `QAOABaseBackendCloud` and `QAOABaseBackendParametric`.

## Recycling Backends from other plugins

Sometimes, a hardware provider supports circuit construction and job execution via an already existing software framework, for example, `Qiskit`. In such a case, the backend plugin may import the `QAOAQiskitQPUBackend` class from the `openqaoa-qiskit` package to be used to support the new plugin instead of re-writing the whole class from scratch. Be sure to add `openqaoa-qiskit` as a requirement for the new plugin if this import is used. 

In an another situation, a backend may permit using `Qiskit` to define the circuit and then using some existing converters to transform the circuit into instructions parseable by the hardware. In such a case, the user may consider writing a new QAOA Backend class that inherits from the `QAOAQiskitQPUBackend` instead of the base-backend classes defined in `openqaoa-core`.