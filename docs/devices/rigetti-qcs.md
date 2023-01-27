# Rigetti Quantum Cloud Services

Quantum Cloud Services (QCS) is Rigetti’s quantum cloud computing platform. If you don't have a QCS account, you can request one [directly from Rigetti](https://www.rigetti.com/get-quantum).


## Installation and Getting Started

QCS gives access to
- All Rigetti's QPU
- The quantum virtual machine (QVM), a quantum computer simulator

Further information can be found in [Rigetti's official documentation](https://docs.rigetti.com/qcs/)

## Explicitly create the QVM backend

In order to instantiate to connect to Rigetti QCS, *you need to be using Rigetti's JupyterLab IDE*. That is, you need to be physically running your code _within_ QCS. As of writing, there is no option for remote access

```Python
rigetti_device = create_device(location='qcs', name='Aspen-M-3')
q_pyquil.set_device(rigetti_device)
```