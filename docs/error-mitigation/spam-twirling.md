# SPAM Twirling 

State Preparation and Measurement (SPAM) Twirling is a simple error mitigation technique used to remove any preferred direction due to readout noise.  It works by diagonalizing the noise associated with measurement by randomly flipping qubits via X gates right before the measurement and flipping the corresponding measured bit classically. 
A calibration factor from the diagonal noise channel is obtained by measuring quantum circuits initialized in the zero states. As a result, the expectation values of the circuit in the presence of readout noise is much closer to the noise-free one.

!!! info "For the curious"
    The technique has been developed theoretically by E. v. d. Berg et al. [1] and by A. W. R. Smith, et al. [2] and implemented within Qiskit as one of their “resilience levels” under the name “Twirled Readout Error eXtinction (TREX)”. Experimentally has been applied by Layden et al. [3] to ensure that the  symmetry requirement $Q(s'| s) = Q(s |s')$ is met. Note that this requirement is specific to the Monte Carlo algorithm and therefore irrelevant to QAOA. 


### The steps to perform SPAM Twirling are:
1. Obtain calibration data (ideally over the whole device) under BFA (subroutine)
1.1. Initialize the circuit in the |000…0> state.
1.2. Measure under BFA (subroutine)
1.3. Save the counts and the registers (this is how the measurement string outcomes map to the physical qubits on the device) and all other relevant information to a json. 
2. Run the quantum experiment under BFA 
2.1. Calculate calibration factors
2.2. Prepare the desired quantum circuit
2.3. Measure under BFA (subroutine)
2.4. Correct the expectation values by term
2.5. Combine all corrected terms into the final expectation value

##### Subroutine: Bit Flip Averaging (BFA) technique

1. Divide the total number of shots into n batches and for each batch choose a set of qubits to be flipped. 
As a side note, if we know the structure of the noise, we should choose the schedule such that it mimics the syndromes of the noise matrix. In particular, for uncorrelated noise on 3 qubits, one must bitflip [[], [0], [1], [2], [0, 1], [0, 2], [1, 2], [0, 1, 2]] which is a variation of combinations, n_batches = 2**3 = 8. Performing those on the |0000..0> state allows for inhering the noise probabilities, i.e. obtaining M and then inverting it to correct the noisy measurement outcome coming from a real experiment. 
Since we usually don’t have a very accurate model of the noise and to keep it general, we choose the schedule completely at random. This is also what the authors in the relevant papers suggest.
2. The bit flip before the measurement can be done by applying an X gate, absorbing the X gate into the last layer of RX rotations or propagating it to the front (and then absorbing it into the initial |+> state). 
3. The bit flip after the measurement is performed by applying a classical not to the outcome bitstring, which is implemented as changing the keys in the count dictionary. 
As a side note, recall that Qiskit stores the measurement outcomes in reversed order, so one must be careful of how it keeps track of the negated qubit indices. Also, as a result of routing, the physical qubits might be swapped. However, both cases are taken care of before returning the get_counts method, so technically we should not care about these. 
4. Lastly, combine the negated counts from all batches into a single count dictionary which is then used for calculating the calibration factors or estimating the expectation values.


![circuit_original](/img/spam_twirling_circuit_0.png)
![circuit_XX](/img/spam_twirling_circuit_1.png)
![circuit_XI](/img/spam_twirling_circuit_2.png)
![circuit_IX](/img/spam_twirling_circuit_3.png)

### How to do this within OpenQAOA?

In the workflows, this can be implemented simply by:
```Python
q.set_error_mitigation_properties(error_mitigation_technique='spam_twirling', n_batches=4, calibration_data_location='filename')
```

Internally, this wrap the backend to allow for dividing the computation in batches where a different set of qubits will be negated.
```Python
if(self.error_mitigation_properties.error_mitigation_technique == "spam_twirling"):
        self.backend = SPAMTwirlingWrapper(
            backend=self.backend,
            n_batches=self.error_mitigation_properties.n_batches,
            calibration_data_location=self.error_mitigation_properties.calibration_data_location,
            )
```

The BaseWrapper class can be ...

## References
----------
1. [Berg, E., Minev, Z. K., Temme, K.](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.105.032620), Phys. Rev. A 105, 032620 (2022) 
2. [Smith, A.W.R., Khosla, K.E., Self, C.N., Kim, M.S.](https://www.science.org/doi/10.1126/sciadv.abi8009),  Science Advances, vol. 7, 47 (2021)
3. [Layden, D., Mazzola, G., Mishmash, R. V., Motta, M., Wocjan, P., Kim, J.-S., Sheldon, S.](https://arxiv.org/abs/2203.12497), arXiv:2203.12497, (see Appendix IV, sec.A2)
