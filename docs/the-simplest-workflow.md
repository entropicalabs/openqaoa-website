# The simplest OpenQAOA workflow

The key concept behind OpenQAOA is workflows. Workflows are intuitive and programmatic ways to build complex QAOAs while focusing more on the actual problem you want to solve. Another way to say the same thing is that you don't have to write the circuit explicitly but rather focus on the keywords and concepts that lie behind the algorithm.

Let's check out the simplest QAOA workflow in action!

First, we need to create a problem instance. For example, an instance of vertex cover:

```Python
import networkx
from openqaoa.problems import MinimumVertexCover
g = networkx.circulant_graph(6, [1])
vc = MinimumVertexCover(g, field=1.0, penalty=10)
qubo_problem = vc.qubo
```
!!! note "QUBO"
    QUBO stands for _Quadratic unconstrained binary optimization_, and a QUBO problem represent, loosely speaking, a binary problem with at most quadratic terms. In general any optimisation problem can be cast as a binary problem, but not all of them will be quadratic in the resulting binary variables. For a very nice paper showcasing most common QUBOs please check out Andrew Lucas's paper [Ising formulations of many NP problems](https://arxiv.org/abs/1302.5843)


Now that we have the qubo problem, we can create a workflow

```Python
from openqaoa import QAOA  

q = QAOA()
q.compile(qubo_problem)
q.optimize()
```

Let's break down the process. First, we create the `QAOA()` object

```Python hl_lines="3"
from openqaoa import QAOA  

q = QAOA()
q.compile(qubo_problem)
q.optimize()
```

The `QAOA()` object is a workflow and contains all the details required to successfully build the circuit and execute it. However, even while using the default values the `QAOA()` object needs to take as input the problem statement

!!! note "Compilation and QUBOs"
    To build the QAOA ansatz we need the cost function, and the cost function is encoded in the qubo problem! :-)

```Python hl_lines="4"
from openqaoa import QAOA  

q = QAOA()
q.compile(qubo_problem)
q.optimize()
```

Compiling the problem is a crucial step: now the representation of the QAOA workflow includes all the key information to build the circuit. So, all we are left with is the last step: the optimization!

```Python hl_lines="5"
from openqaoa import QAOA  

q = QAOA()
q.compile(qubo_problem)
q.optimize()
```

Now that we have run a quantum algorithm in a few lines of code, we shall try to make sense of the result.