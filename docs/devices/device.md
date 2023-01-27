# Devices

Here are all the devices supported by OpenQAOA. Please note that the list is _alphabetically_ sorted.

In OpenQAOA devices are divided between local and cloud base.


# Clouds:
* [Amazon braket](amazon-braket.md)
* [Azure quantum](azure-quatum.md)
* [IBM Quantum](ibmq.md)
* [Rigetti Quantum Cloud Services](rigett-qcs.md)


# Local:
* [Qiskit](qiskit.md)
* [Rigetti QVM](rigetti-qvm.md)
* [Vectorized](entropica-labs-vectorized.md)


Simulators further divide into two broad categories:
- Statevector or Wavefunction simulators: the full wavefunction of the circuit is computed. That is, $2^n$ complex numbers are returned to the user. 
- Shot based simulators: return a list of measurement outcomes after sampling the wavefunction.
