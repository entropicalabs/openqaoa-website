# Devices

OpenQAOA gives access to a large set of local simulators, and Quantum Computers hosted on public (and private) clouds.

Before going through the details of each provider, please note that  **financial costs** may be associated to the execution of quantum (and classical) computations on public (and private) cloud. Be careful and always make sure you know what you are doing before trying to send a computation to a third party! 

Conversely, all the local simulators run on your own machine and there is no extra cost associated.

!!! info "Types of Simulators"

    Simulators further divide into two broader categories:

    - Statevector or Wavefunction simulators: the full wavefunction of the circuit is computed. That is, $2^n$ complex numbers are returned to the user. 
    - Shot based simulators: return a list of measurement outcomes after sampling the wavefunction.

The Device Object is OpenQAOA's way of organizing the various methods of authenticating and validating with a particular cloud provider. Which, in turn, gives us access to the various QPUs and cloud-hosted Simulators.

OpenQAOA divides the Devices into 2 categories: Local and Cloud. 

* A Local device is one that runs on the user's local machine and does not require any connection to a remote cloud. 
* A Cloud device is one that requires a connection to an external cloud provider.

The following are a list of Local and Cloud devices currently available through OpenQAOA. Please note that the list below is simply _alphabetically_ sorted.

# Cloud:
* [Amazon braket](amazon-braket.md)
* [Azure quantum](azure-quantum.md)
* [IBM Quantum](ibmq.md)
* [Rigetti Quantum Cloud Services](rigetti-qcs.md)


# Local:
* [Qiskit](qiskit.md)
* [Rigetti QVM](rigetti-qvm.md)
* [Vectorized](entropica-labs-vectorized.md)


## Properties of devices

When using a OpenQAOA workflow, under the hood a device object is created. 

```Python
q = QAOA()

my_device = create_device(location='***' 
                          name='***')
q.set_device(my_device)
```

The device is then accessible in memory as an attribute of `q`. That is, try typing `q.device` to access it, and see all its properties.

### Local devices

Local devices can be created by specifying the `location` input parameter as `'local'`.

In the following example, we create a device object compatible with the vectorized backend.

```Python
my_device = create_device(location='local' 
                          name='vectorized')
```

### Cloud devices

Cloud devices, on the other hand, have a `location` input parameter that is dependent on the cloud provider. 

You can use the following table for your reference:

| Cloud Provider Name | `location` |
|---------------------|------------|
| Amazon Braket       | `aws`      |
| Azure quantum       | `azure`    |
| IBMQuantum          | `ibmq`     |
| Rigetti Quantum Cloud Services | `qcs` |

The format of the `name` input parameter also varies depending on which cloud provider is being used. Furthermore, each cloud device would have to be created with the appropriate credentials or the user would have to be authenticated through the various cloud provider CLI tools before the device can be created.

Check in the individual device pages on how to create the respective devices. 