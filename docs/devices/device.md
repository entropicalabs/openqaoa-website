# Devices

OpenQAOA gives access to a large set of local simulator, and Quantum Computer hosted on public (and private) clouds.

Before going through the details of each provider, please note that  **financial costs** may be associated to the execution of quantum (and classical) computations on public (and private) cloud. Be careful and always make sure you know what you are doing before trying to send a computation to a third party! 

Conversely, all the local simulators run on your own machine and there is no extra cost associated.


In OpenQAOA devices are divided between local and cloud base. Please note that the list below is simply _alphabetically_ sorted

# Clouds:
* [Amazon braket](amazon-braket.md)
* [Azure quantum](azure-quantum.md)
* [IBM Quantum](ibmq.md)
* [Rigetti Quantum Cloud Services](rigetti-qcs.md)


# Local:
* [Qiskit](qiskit.md)
* [Rigetti QVM](rigetti-qvm.md)
* [Vectorized](entropica-labs-vectorized.md)


Simulators further divide into two broad categories:
- Statevector or Wavefunction simulators: the full wavefunction of the circuit is computed. That is, $2^n$ complex numbers are returned to the user. 
- Shot based simulators: return a list of measurement outcomes after sampling the wavefunction.


## Properties of devices

When using a OpenQAOA workflow, under the hood a device object is created. 

```Python
q = QAOA()

my_device = create_device(location='***' 
                          name='***')
q.set_device(my_device)
```

The device is then accessible in memory as an attribute of `q`. That is, try typing `q.device` to access it, and see all its properties.