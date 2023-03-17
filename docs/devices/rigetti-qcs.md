# Rigetti Quantum Cloud Services

Quantum Cloud Services (QCS) is Rigetti’s quantum cloud computing platform. If you don't have a QCS account, you can request one [directly from Rigetti](https://www.rigetti.com/get-quantum).

!!! note "QPU accessibility"
    As of writing, Rigetti devices are available **only** through QCS. That is, you have to run OpenQAOA within a notebook hosted on QCS. You can sign in from the [official QCS website](https://qcs.rigetti.com/sign-in)


## Installation and Getting Started

QCS gives access to
- All Rigetti's QPU
- The quantum virtual machine (QVM), a quantum computer simulator

Further information can be found in [Rigetti's official documentation](https://docs.rigetti.com/qcs/)

## Explicitly create the QVM backend

In order to instantiate to connect to Rigetti QCS, *you need to be using Rigetti's JupyterLab IDE*. That is, you need to be physically running your code _within_ QCS. As of writing, there is no option for remote access

```Python
from openqaoa import QAOA, create_device

q_pyquil = QAOA()

rigetti_device = create_device(location='qcs', name='Aspen-M-3')
q_pyquil.set_device(rigetti_device)
```