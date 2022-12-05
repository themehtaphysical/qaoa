# Minimum Vertex Cover Probelm using QAOA

## Input

A new graph can be created using the graph class within our program. This class can take a dictionary representing a graph upon initialization, i.e. test_graph = graph(graph_dictionary). Or the graph class can take no dictionary upon initialization, and, instead, be given a list of lists of all edges later using the make_graph_from_connections function, i.e. test_graph.make_graph_from_connections(edge_list). The function QAOA_Vertex_Cover(connections, gamma, beta) is used to create the quantum circuit for the minimum vertex cover problem for a given graph. The connections of a graph from the graph class can be acquired with the member function getEdges(), i.e. test_graph.getEdges(). Gamma and beta are the values for the rotations performed by the separation and mixer rotation matrices used in the QAOA circuit and can be specified by the user. These values will be found in the optimization process later to find the best solution, though choosing good initial values is still important, as the optimizer can get stuck in a local minimum for some inputs. After making the circuit, you can follow the usual steps to run the QAOA algorithm on a graph: pick a backend, transpile the circuit for your chosen backend, run the circuit on the chosen backend for a given number of shots, and print out the counts for each output. The meaning of these outputs is explained below. 

## Running the program

The actual program and algorithm require you to first run the QAOA circuit, and use the output to optimize the gamma and beta values. For optimizing the gamma and beta values, we use scipy.optimize.minimize. This function takes a function to optimize as the first parameter, a list of values to optimize as the second, and the desired optimization method as the third parameter. The first parameter of the minimize function can be acquired using the get_expectation(graph, backend) function, which takes in a graph to run the QAOA circuit with, as well as the backend that you wish to use for running the circuit. This function returns a function that runs the quantum circuit and calculates the expectation value of the problem hamiltonian using the objective function. The second parameter will need to be a list that has your chosen initial gamma value in the 0th position and your chosen initial beta value in the 1st position. These values will be optimized using the expectation function, which you can then use to run the circuit again to acquire your approximate solution to the minimum vertex problem.

## Understanding the Output

Each bit in the output represents a vertex. If a bit is zero, then the vertex isn’t marked in the solution to the problem; if it’s one, then the vertex is marked. The most likely outcomes are cases in which the combination of marked and unmarked vertices is a solution to the vertex cover problem. There are cases in which there are multiple solutions (as seen in the 5 vertex case above). However, due to the nature of QAOA and this problem specifically, some outputs might not be a solution. This occurs because the conditions for satisfying the problem (including all edges) and using the fewest vertices are weighted equally in the Count operator. Therefore, a “check” might be required to ensure that the output is indeed a solution.


## Citation

> https://doi.org/10.1016/j.asoc.2022.108554
