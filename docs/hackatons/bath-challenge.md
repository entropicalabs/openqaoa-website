# Bath Challenge

Welcome to the OpenQAOA challenge!

Our challenge is to solve the [Graph Colouring problem](https://en.wikipedia.org/wiki/Graph_coloring), one of the famous NP-complete problems, with a quantum computer.

NP stands for [nondeterministic polynomial time](https://en.wikipedia.org/wiki/NP_(complexity)), and it is a set of problems for which the time required to find a solution grows exponentially with the problem size, but the time it takes to verify the correctness of a solution is polynomial. Other famous examples are the travelling salesperson problem and many other graph problems.

One way to tackle NP problems is to employ heuristics. The **quantum approximate optimization algorithm** is one such heuristic algorithm, and it can be used to solve (small!) binary optimization problems. 

One key feature of QAOA is that it can be run on existing quantum computers! 

The original QAOA paper can be found on the arxiv ([Farhi, Edward, Jeffrey Goldstone, and Sam Gutmann. "A quantum approximate optimization algorithm](https://arxiv.org/abs/1411.4028)) but if you need a more lay-down intro, please check out the OpenQAOA docs [what-is-the-qaoa](docs/what-is-the-qaoa.md), and many more can be found on the internet.


## How to approach the challenge
Quantum computations are not easy. So, we broke the challenge into a series of sequential steps. We also left out some extra research avenues in case there is extra time, or if you want to continue exploring the problem past the challenge date :


To solve this challenge you will need to:

##  The cost function
Find the objective/cost function for the graph colouring problem. QAOA requires you to write the cost function in terms of binary variables. However, it turns out that is not too complicated for graph-based problems. To be more precise, the encoding that you need to prepare must be an **ising encoding**. Do not worry if you don't know what an ising encoding is, fundamentally it simply means that the binary encoding must be ${-1,+1}$.

A good place to familiarise yourself with these ising cost functions is the great paper [Ising formulations of many NP problems](https://arxiv.org/abs/1302.5843) by Andrew Lucas. In section $6.1$ he introduces graph colouring problems, and it offers the following cost function

$$
A \sum_v(1 - \sum_{i=1}^{n} x_{v,i})^2 + A \sum_{(uv) \in E} \sum_{i=1}^{n} x_{u,i}x_{v,i}
$$

Check out the paper for more information :) 

!!! hint
    If you are stuck, and you need some inspiration you can check how some of the other problem classes are structured in OpenQAOA by checking out the [OpenQAOA github page](https://github.com/entropicalabs/openqaoa/tree/dev/openqaoa/problems). In particular, note that in some cases we wrote the cost function using the binary variables $0,1$ and converted them to Ising variables by invoking the method `get_qubo_problem()`

## Solve the Graph coloring problem using OpenQAOA
Check out the OpenQAOA workflows (for a refresher, check out [the-simplest-workflow](docs/the-simplest-workflow.md) and [customise-the-QAOA-workflow](docs/workflows/customise-the-QAOA-workflow.md))

Then all you need to do, is to create the right QUBO 

```Python
graph_coloring_qubo = QUBO(terms=[[...]], weights)
```

## Study the QAOA parameters

Our goal is to achieve the lowest cost value. How does the lowest cost change as we vary certain QAOA Parameters? 

What happens when you vary the number of layers $p$? And as you increase the number of verteces? What can you tell about the size of the graph? And is there anything special when it comes to the initialization parameters? 

You can easily test many of these parameters through OpenQAOA workflow setters:

```Python
q = QAOA()
q.set_circuit_properties(p=3, param_type='fourier', q=2)
```

Can you find a combination of QAOA Parameters that gives you the best result for any random graph?

## Study the Graph
Plot a scaling of the performance of the algorithm as a function of changing graph properties (e.g. edge connectivity)

## Extras 
Can you come up with an heuristics for the parameter initialization?
