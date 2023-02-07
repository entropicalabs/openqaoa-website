# What is the QAOA

Quantum computing has the potential to revolutionize the way we solve complex problems, and the Quantum Approximate Optimization Algorithm (QAOA) is one such algorithm that has been gaining attention in recent years. QAOA is a hybrid quantum-classical algorithm that combines quantum circuits and classical optimization to find approximate solutions to combinatorial optimization problems.

Combinatorial optimization is the process of finding the best solution from a finite set of possible solutions, and it plays a crucial role in fields such as machine learning, finance, and logistics. QAOA provides a new and innovative approach to solving these problems, using quantum computers to explore the solution space more effectively.


## Description of the algorithm

The algorithm consists of two main components: a parametric quantum circuit and classical optimization.

The parametric quantum circuit consists of gates that encode the problem to be solved, and these gates are determined by parameters that need to be optimized. The evaluation of the circuit yields the most probable state, which serves as a potential solution to the optimization problem.

The classical optimization component of the algorithm involves using classical algorithms to optimize the parameters of the quantum gates. This optimization process is performed iteratively, where each iteration involves reapplying the quantum gates to the quantum state using the latest set of optimized parameters, until a near-optimal solution is found.


``` mermaid
graph TB
  A[Encode problem in parametric quantum circuit] --> B[Choose initial parameters];
  B --> C[Evaluate the circuit];
  C --> D{Are parameters optimized?};
  D -- No --> E[Optimize parameters with classical algorithms];
  E --> C;
  D -- Yes --> F[Output near-optimal solution];
```


## The quantum circuit

The starting state of the quantum circuit is $|0\rangle^{\otimes n}$, where $n$ is the number of qubits being used. The state after the circuit is applied is given by:

$$|\psi(\gamma,\beta)\rangle = \mathcal{U}(\gamma,\beta)\,|0\rangle^{\otimes n},$$

where ${\gamma}={\gamma_1, \gamma_2, ..., \gamma_p}$ and ${\beta}={\beta_1, \beta_2, ..., \beta_p}$ are two sets of $p$ parameters.

The circuit of the QAOA algorithm is defined as follows:

$$\mathcal{U} (\gamma,\beta) = U(\mathcal{H}_X,\beta_p)U(\mathcal{H}_C,\gamma_p) ... U(\mathcal{H}_X,\beta_1)U(\mathcal{H}_C,\gamma_1) \, H^{\otimes n} ,$$

where $H$  is a Hadamard gate, $U(\mathcal{H}_X,\beta_j) = e^{-i\beta_j \mathcal{H}_X}$ and $U(\mathcal{H}_C,\gamma_j) = e^{-i\gamma_j \mathcal{H}_C}$.

$\mathcal{H}_C$ is the cost Hamiltonian, which encodes the problem to be solved, and $\mathcal{H}_X$ is the mixing Hamiltonian:

$$\mathcal{H}_X = \sum_{i=1}^n X_i, $$

where $X_i = I_1\otimes...\otimes I_{i-1} \otimes \sigma_{i}^x \otimes I_{i+1} \otimes...\otimes I_{n}$. Here, $\sigma_i^x$ are the Pauli X matrices.

The goal of the QAOA algorithm is to find the optimal sets of angles ${\gamma^{\text{opt}}}$ and ${\beta^{\text{opt}}}$ that minimize $\langle\psi|\mathcal{H}_C|\psi\rangle$. With the optimal parameters, $|\psi(\gamma,\beta)\rangle$ is a superposition of base states, and the state with the highest probability will be the solution to the problem.

The optimization of the parameters is carried out iteratively using a classical optimization algorithm such as cobyla or gradient descent.

For $p\to \infty$, it has been proven that a set of optimal parameters exist such that the solution is exact. However, for smaller values of $p$, the solution is only an approximation.

## Encoding of the problem

The encoding of the problem is a crucial step in the QAOA algorithm, as it determines the form of the parametric quantum circuit. This encoding is done by representing the problem as a cost Hamiltonian, $\mathcal{H}_C$, which maps the problem's variables to qubits and defines the cost function as an operator that acts on these qubits.

Problems that can be tackled by QAOA include Minimum Vertex Cover, Maximum Cut, Shortest Path, Number Partition, etc. These problems can all be mapped into a QUBO (Quadratic Unconstrained Binary Optimization) problem.

A QUBO problem consist on finding a vector $x$ such that the function

$$ f(x) = \sum_{i=1}^n \sum_{j=1}^i Q_{i j} x_i x_j + C $$

is minimized. 
Where $x$ is a vector of $n$ components, and $Q_{ij}$ and $C$ are constants. Each different problem will have different $Q_{ij}$ and $C$ values.

The function can be enconded in a cost Hamiltonian the following way:

$$ \mathcal{H}_C = \sum_{i=1}^n \sum_{j=1}^i Q_{i j} \sigma_i^z \sigma_j^z + \sum_{i=1}^n C \sigma_i^z $$

where $\sigma_i^z$ are Pauli Z matrices.

## Decomposition of the circuit 

As the terms of these Hamiltonians commute, we can write:

$$U(\mathcal{H}_X,\beta_j) = e^{-i\beta_j \mathcal{H}_X} = \prod_k^n e^{-i\beta_j \sigma_{k}^x}, $$

$$U(\mathcal{H}_C,\gamma_j) = e^{-i\gamma_j \mathcal{H}_C} = \prod_k^n\prod_l^k e^{-i\gamma_j Q_{kl} \sigma_{k}^z\sigma_{l}^z}\,\prod_k^n e^{-i\gamma_j C \sigma_{k}^z}.$$

We know that $e^{-i\alpha \sigma_{k}^x}$ is the RX gate of $2\alpha$ applied at the $k$ qubit, $e^{-i\alpha \sigma_{k}^z}$ is the RZ gate of $2\alpha$ applied at the $k$ qubit, and $e^{-i\alpha \sigma_{k}^z\sigma_{l}^z}$ is the RZZ gate of $2\alpha$ applied at the $k$ and $l$ qubits.

This means that we now know how to construct the QAOA circuit for any QUBO problem.

## Classical loop

The algorithm proceeds the following way:

Once we have encoded the problem to solve in a parametric quantum circuit, we have to choose some initial parameters $\gamma^\text{ini}, \beta^\text{ini}$. Then evaluate $\langle \psi|\mathcal{H}_C|\psi\rangle $ and choose a new set of parameters using a classical optimizer. 

The classical optimizer algorithm can be gradient-free such us cobyla or it can use a gradient algorithm such as gradient descent









