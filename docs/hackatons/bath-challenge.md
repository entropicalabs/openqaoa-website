# Bath Challenge

Welcome to the OpenQAOA challenge!

Our challenge is to solve the [Graph Colouring problem](https://en.wikipedia.org/wiki/Graph_coloring), one of the famous NP-complete problems, with a quantum computer.

From the good old [wikipedia page](https://en.wikipedia.org/wiki/NP_(complexity)), in computational complexity theory, NP (nondeterministic polynomial time) is a complexity class used to classify decision problems. NP is the set of decision problems for which the problem instances, where the answer is "yes", have proofs verifiable in polynomial time by a deterministic Turing machine, or alternatively the set of problems that can be solved in polynomial time by a nondeterministic Turing machine.

One way to tackle NP problems is to employ heuristics. The **quantum approximate optimization algorithm** is one such heuristic algorithm, and it can be used to solve (small!) binary optimization problems. What interests us today, is that QAOA is an algorithm that can be run on existing quantum computers! 

The original QAOA paper can be found on the arxiv ([Farhi, Edward, Jeffrey Goldstone, and Sam Gutmann. "A quantum approximate optimization algorithm](https://arxiv.org/abs/1411.4028)). The paper is a bit technical, and may not be the best reference for a 12h challenge. So, if you need a more lay-down intro, please check out the OpenQAOA [what-is-the-qaoa](/docs/what-is-the-qaoa.md) reference. There are also a lot of great references out there on the web.


## **How to approach the challenge**
Quantum computations are not easy. So, we broke the challenge into a series of sequential steps. 

You don't need to strictly follow the steps as we have outlined them. The most important things are:

 - A correct implementation of the graph coloring problem
 - Your exploration of the problem space!

The last section includes some ideas on how to further explore the challenge.


##  **The cost function**

First things first: what are we trying to do? We are given a graph with $N$ vertexes and some connectivity, and a set of $n$ colors. The optimization problem then is to find a color for each vertex such that no edge connects two vertexes of the same color. Two simple examples are given by the following choices of graphs and coloring: 

![graph_coloring](/img/Greedy_colourings.svg.png)

Now that we have an idea of the problem that we are trying to solve, we need to find the objective/cost function for the graph colouring problem. QAOA requires you to write the cost function in terms of **binary variables**. However, it turns out that is not too complicated for graph-based problems. 

OpenQAOA requires what is, among physicists, often known as the **ising encoding**. Do not worry if you don't know what an ising encoding is, it simply means that the binary encoding must be ${-1,+1}$. You can check out the [what-is-a-qubo](/docs/problems/what-is-a-qubo.md).

A good place to familiarize yourself with these ising cost functions is the great paper [Ising formulations of many NP problems](https://arxiv.org/abs/1302.5843) by Andrew Lucas. In section $6.1$ he introduces graph colouring problems, and it offers the following cost function

$$
A \sum_v(1 - \sum_{i=1}^{n} x_{v,i})^2 + A \sum_{(uv) \in E} \sum_{i=1}^{n} x_{u,i}x_{v,i}
$$

We left any detailed explanation of the terms in the above equation just to make sure you do check out Andrew Lucas paper ;)

!!! hint
    If you are stuck, and you need some inspiration you can check how some of the other problem classes are structured in OpenQAOA by checking out the [OpenQAOA github page](https://github.com/entropicalabs/openqaoa/tree/dev/openqaoa/problems). In particular, note that in some cases we wrote the cost function using the binary variables $0,1$ and converted them to Ising variables by invoking the method `get_qubo_problem()`


## **Solve the Graph coloring problem using OpenQAOA**

Check out the OpenQAOA workflows (for a refresher, check out [the-simplest-workflow](docs/the-simplest-workflow.md) and [customise-the-QAOA-workflow](docs/workflows/customise-the-QAOA-workflow.md))

Then all you need to do, is to create the right QUBO and pass the terms and weights given by the cost function above.

```Python
graph_coloring_qubo = QUBO(terms=[[..], [..], ...],
                           weights=[...],
                           n=number_of_vertexes)
```

Note that `terms` is a list of lists, and `weight` a list.

!!! tip "Start with a simple problem"
    Make sure you start with a small graph and remember to make sure that `len(terms) == len(weights)`!

Then, all you need to do for your first QAOA tests is to solve use a QAOA workflow:

```Python
q = QAOA()
q.compile(graph_coloring_qubo)
q.optimize()
```

After the optimization make sure you explore and familiarize yourself with the `q.results` object! 

!!! danger "Try not to crash your laptop!"
    Remember that to simulate a quantum computer you need $2^{n}$ variables, where $n$ is the number of qubits. In our case this means that given $n$ colors and $N$ vertexes you will need $2^{nN}$ numbers. Typically each number is represented by 128bits. You can try to plot $128 * 2^{nN}$ to see how quickly you will run out RAM! 

## **Interpret the result**

So, you ran a QAOA workflow successfully ... what next? Well, you can start studying the `q.results` object. First, read the [making-sense-of-the-result](docs/making-sense-of-the-result.md) page. 

The result will look at so
```Python
> results
{'solutions_bitstrings': ['010110'], 'bitstring_energy': -4.0}
```

This represents the most probable quantum state identified by the QAOA. In order to figure out how to interpret this result, you need to go back to the cost function. You need to make sure you know how to map each binary variable within the bistring back to the binary variable $x_{v,i}$!


## **Study the QAOA parameters**

Our goal is to achieve the lowest cost value. How does the lowest cost change as we vary certain QAOA Parameters? 

What happens when you vary the number of layers $p$? And as you increase the number of vertexes? What can you tell about the size of the graph? And is there anything special when it comes to the initialization parameters? 

You can easily test many of these parameters through OpenQAOA workflow setters:

```Python
q = QAOA()
q.set_circuit_properties(p=3, param_type='fourier', q=2)
```

Can you find a combination of QAOA Parameters that gives you the best result for any random graph?

## **Study the Graph and the Solution**

Andrew Lukas paper says that given `n` colors and `N` vertexes it takes `nN` qubits to encode the problem. Without saturating your RAM, do you think you can play around with different graph topologies and colouring schemes?

Maybe try to plot a scaling of the performance of the algorithm as a function of changing graph properties (e.g. edge connectivity).

## **Extras** 

In the eventuality that you have had the time to explore QAOA and the graph colouring problem, here are a few ideas to keep you entertained:

- Try to solve the problem using a Quantum Computer! We suggest you use the free IBM ones, see the [ibmq](docs/devices/ibmq.md) page
- Can you come up with some interesting parameter initialization strategies?
- Have a look at [RQAOA](docs/workflows/recursive-qaoa.md): how does it perform with respect to plain QAOA?
 
