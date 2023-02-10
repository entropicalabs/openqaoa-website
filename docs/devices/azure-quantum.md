# Azure Quantum

OpenQAOA supports all gate-based QPU present on Azure Quantum. For the freshest list, please check the [Azure Quantum documentation](https://azure.microsoft.com/en-us/products/quantum/#features)

Currently, the available devices are

| Item         | Device Name  | Paradigm	 |  Type    | Target   | # Qubits |
|--------------|--------------|--------------|----------|--------------|--------|
| IonQ      |  Quantum simulator |gate-based | SIM | ionq.simulator         | 29 |
| IonQ      |  IonQ Harmony      |gate-based | QPU | ionq.qpu	            | 11 |
| IonQ      |  IonQ Aria         |gate-based | QPU | ionq.qpu.aria-1        | 23 |
| Rigetti   |  QVM               |gate-based | SIM | rigetti.sim.qvm        | -  |
| Rigetti   |  Aspen-M-2         |gate-based | QPU | rigetti.qpu.aspen-m-2	| 80 |
| Rigetti   |  Aspen-M-3         |gate-based | QPU | rigetti.qpu.aspen-m-3	| 80 |
| Quantinuum|  H1-1              |gate-based | QPU | quantinuum.qpu.h1-1	| 20 |
| Quantinuum|  H1-2              |gate-based | QPU | quantinuum.qpu.h1-2	| 12 |
| Quantinuum|  H1-1 Emulator     |gate-based | QPU | quantinuum.sim.h1-1e	| 20 |
| Quantinuum|  H1-2 Emulator     |gate-based | QPU | quantinuum.sim.h1-2e	| 12 |


!!! danger
    **Most computations on Azure Quantum carry a financial cost**. Please, make sure you are conformable with the latest [Amazon Braket pricing model](https://docs.aws.amazon.com/braket/latest/developerguide/braket-pricing.html) before continuing! 


## How to connect to Azure Quantum

There are two ways to access the devices on Azure Quantum you will need to create an Azure Quantum account. You can do so from the [official Azure website](https://azure.microsoft.com/en-us/products/quantum/)

### Access through your own laptop

In order to access Azure Quantum services from your own device you will need to authenticate yourself locally. The simplest way to authenticate is to use the [Azure Command Line Interface](https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli)

To install the Azure CLI, please refer to the [official documentation](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

### Access through Azure Quantum hosted notebooks

You can get started by following [Azure Quantum official docs](https://learn.microsoft.com/en-us/azure/quantum/how-to-create-workspace?tabs=payg%2Ctabid-quick). This will allow you to create  an Azure Quantum workspace where you can access Azure hosted notebooks.


#### Creating an Azure device

The procedure is simple:

```Python
q = QAOA()

device_azure = create_device(location='azure',
                             name='ionq.simulator',
                             resource_id="/subscriptions/****/resourceGroups/****/providers/****/Workspaces/****",
                             az_location='westus')
q.set_device(device_azure)
```

Please, note that you need to specify the correct path for your `resource_id` and select the correct `az_location`!
