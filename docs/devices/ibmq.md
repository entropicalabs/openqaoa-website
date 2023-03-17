# IBM Quantum

OpenQAOA supports all gate-based QPUs present on IBMQ Experience. OpenQAOA also supports remote simulators like the ibmq qasm simulator and statevector simulator. For the latest list of available QPUs and simulators, log into your IBMQ Experience account [here](https://quantum-computing.ibm.com/). Once logged in, click on the navigation menu icon on the top left and go to "Compute Resources".

Currently, some available simulators are:

| Name | Qubits | Processor-type | Plan |
|------|--------|----------------|------|
| ibmq_qasm_simulator | 32 | General, Context Aware | open |
| simulator_statevector | 32 | Schrödinger wavefunction | open |


!!! info
    QPUs classified under the "Premium" plan requires paid access. For free-tiered QPUs, use QPUs that are classified as "open".
    
## How to connect to IBMQ

There are two ways to access the devices on IBMQ:

### Access through your own laptop

In order to access IBMQ devices through your local computer, you will need to retrieve your IBMQ API Token. Your account API Token can be found on your IBMQ account page [here](https://quantum-computing.ibm.com/account)
    
OpenQAOA utilizes the Qiskit library to authenticate with IBMQ. As such, the user is first required to save his API Token, locally, through Qiskit.

!!! danger
    Since Qiskit requires the API Tokens to be used for programmatic access. Be cautious about where you expose your code. If leaked, they may compromise your IBMQ account.

```Python
from qiskit import IBMQ

IBMQ.save_account('YOUR_API_TOKEN')
```

OpenQAOA then loads and authenticates the saved API Token via Qiskit.

### Access through IBMQ hosted notebooks

You can also use the hosted notebooks on IBMQ Experience with OpenQAOA. Just install openqaoa using `pip install openqaoa` in the terminal in Quantum Lab.
Once installed, the steps to running a computation in the Quantum Lab is the same as when running OpenQAOA from your own laptop.

#### Creating an IBMQ Device

An IBMQ Device can be created by the following:

```Python
from openqaoa import QAOA, create_device
from qiskit import IBMQ

IBMQ.load_account() # load the previously saved credentials

q = QAOA()

qasm_sim_device = create_device(location='ibmq', 
                                name='ibmq_qasm_simulator',
                                hub='ibm-q', 
                                group='open', 
                                project='main')

q.set_device(qasm_sim_device)
```

#### Find an available Device

If you are unsure about what devices are available to you, you can do the following:

```Python
from openqaoa import create_device

q_device = create_device(location='ibmq', 
                        name='', hub='ibm-q', 
                        group='open', project='main')

q_device.check_connection()
q_device.available_qpus
```

The attribute `check_connection` authenticates your IBMQ credentials while `available_qpus` returns a list of IBMQ devices that are available to your IBMQ account, based on your API Token.

Once you've selected the desired device, recreate the device object with the selected device's name as `name` in the `create_device` function.