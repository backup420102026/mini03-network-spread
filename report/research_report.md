# Mini Research #3: Network Topology and Information Spread

## 1. Research Question

**How does network topology affect the spread of information in a simulated social network?**

## 2. Introduction

Information spreads through connections between individuals. In a social network, however, the existence of connections is not the only relevant factor. The arrangement of those connections can determine whether information remains localized, moves rapidly through the network, or reaches a large proportion of nodes only after many transmission steps.

Network science provides mathematical models that represent different structural patterns. Random networks distribute connections probabilistically, Small-World networks emphasize local clustering together with short paths, and Scale-Free networks contain heterogeneous connectivity in which some nodes have many more connections than others.

This project uses these three synthetic network models to investigate how topology influences simulated information-spread dynamics. The purpose is not to claim that one mathematical network model perfectly represents real society. Instead, the study provides a controlled computational experiment in which the network structure is changed while the main transmission assumptions remain fixed.

## 3. Problem Statement

If two simulated social networks contain the same number of individuals, they can still behave differently because their connections are organized differently. A topology containing highly connected nodes may provide information with routes that are unavailable in a more uniform network. Conversely, strong local clustering may keep information circulating within neighborhoods before it reaches distant parts of the network.

The central problem is therefore to determine whether these structural differences produce measurable differences in the speed or final extent of information spread.

## 4. Objective

The objective is to compare information spreading across Random, Small-World, and Scale-Free network topologies using the same number of nodes, transmission probability, simulation duration, number of repeated runs, and evaluation target.

Two primary outcomes are measured:

1. **Final spread:** the mean percentage of informed nodes after 30 simulation steps.
2. **Time to 50%:** the mean number of simulation steps required to reach at least 50% informed nodes.

## 5. Hypothesis

The hypothesis is that network topology will affect the **speed** of information spread, even if different topologies eventually reach similar final coverage.

## 6. Methodology

### 6.1 Network Size

Each network contains **100 nodes**. A node represents a simulated individual.

### 6.2 Network Models

Three standard synthetic graph models are generated:

- **Random:** Erdős–Rényi graph with `p = 0.05`.
- **Small-World:** Watts–Strogatz graph with `k = 6` and rewiring probability `p = 0.1`.
- **Scale-Free:** Barabási–Albert graph with `m = 3`.

The generated networks contain:

- Random: 224 edges, average degree 4.48, average clustering 0.0423.
- Small-World: 300 edges, average degree 6.00, average clustering 0.4430.
- Scale-Free: 291 edges, average degree 5.82, average clustering 0.1870.

These metrics describe the specific generated graphs used in this experiment.

### 6.3 Information-Spread Model

At the beginning of each run, one node is selected as the initial informed node.

At every simulation step, each currently informed node attempts to transmit information to each uninformed neighboring node. Each attempted transmission succeeds with probability **0.20**.

Newly informed nodes are retained and can transmit information during subsequent steps.

### 6.4 Simulation Parameters

| Parameter | Value |
|---|---:|
| Nodes | 100 |
| Transmission probability | 0.20 |
| Simulation steps | 30 |
| Runs per topology | 30 |
| Target spread | 50% |
| Random seed | 42 |

Each run records the state at steps 0 through 30. Therefore, the total number of observations is:

**3 topologies × 30 runs × 31 recorded steps = 2,790 records.**

## 7. Evaluation Procedure

The final spread is calculated at step 30 for every run and then averaged within each topology.

For the time-to-50% metric, the first simulation step at which a run reaches at least 50% informed nodes is recorded. If a run does not reach the target within the simulation window, it is treated as not reaching the target rather than as zero.

This distinction is important because a missing target should not be interpreted as instantaneous spread.

## 8. Results

### 8.1 Final Spread

| Network | Final Spread |
|---|---:|
| Random | 99.93% |
| Small-World | 100.00% |
| Scale-Free | 100.00% |

After 30 simulation steps, all three topologies reached approximately complete information spread. The Random network averaged 99.93%, while both Small-World and Scale-Free reached 100%.

This indicates that under the selected transmission probability and 30-step window, topology did not produce a substantial difference in final coverage.

### 8.2 Time to Reach 50%

| Network | Mean Time to 50% |
|---|---:|
| Random | 9.00 steps |
| Small-World | 9.20 steps |
| Scale-Free | 5.93 steps |

The largest difference appears in the speed of spread. The Scale-Free network reached the 50% target in an average of 5.93 steps, compared with 9.00 steps for Random and 9.20 steps for Small-World.

Thus, the Scale-Free topology reached the target roughly three simulation steps earlier than the other two models under this experimental setup.

## 9. Analysis and Discussion

The results support the hypothesis within the boundaries of the simulation. The three networks eventually reached almost the same final coverage, but they did not reach that coverage at the same speed.

The Scale-Free network was clearly the fastest according to the time-to-50% metric. A plausible structural interpretation is that its heterogeneous degree distribution contains highly connected nodes that can provide many transmission opportunities. When such nodes become informed, information can potentially reach a larger number of neighbors in fewer steps.

The Random and Small-World networks produced similar mean times to 50%, with the Random network reaching the target at 9.00 steps and the Small-World network at 9.20 steps. Their final spreads were also nearly identical to the Scale-Free result.

The result should not, however, be interpreted as evidence that Scale-Free networks always produce faster information or rumor spread in real societies. The experiment uses a specific synthetic graph, a fixed transmission probability, a limited number of steps, and selected graph-generation parameters.

## 10. Structural Comparison

The generated graphs are not identical in their number of connections. The Random network has 224 edges, while the Small-World and Scale-Free networks have 300 and 291 edges respectively.

This matters because connectivity itself can influence spread. Consequently, the experiment is best described as a comparison of **standard network topology models under their selected generation parameters**, rather than a perfectly isolated test of topology with every structural statistic held constant.

The clustering measurements also differ substantially. The Small-World network has the highest average clustering among the generated graphs, while the Random network has the lowest. The Scale-Free network lies between them for this particular realization.

These differences help explain why the experiment should be interpreted as a topology-model comparison rather than as a claim about a single structural variable.

## 11. Validation

The experiment included structural and numerical validation checks:

- Expected record count: **2,790**.
- Spread values remained within the valid range of **0 to 1**.
- All three generated graphs contained **100 nodes**.
- The result set contained all three expected network types.

These checks passed for the completed experiment.

## 12. Limitations

### Synthetic Environment
The network represents a computational abstraction rather than a measured real-world social network.

### Transmission Probability
The value of 0.20 is a modeling choice. It should not be interpreted as a measured probability of real people sharing information.

### Unequal Edge Counts
The graph-generation parameters produce different numbers of edges across the three networks. This means connectivity is not perfectly controlled.

### Finite Simulation Window
The experiment observes only 30 simulation steps. Different conclusions could appear with a shorter or longer observation window.

### Parameter Sensitivity
Changing network size, transmission probability, topology parameters, or random seed can alter the numerical results.

## 13. Threats to Validity

The main internal validity concern is that topology models differ in more than one structural characteristic. In particular, edge counts and degree distributions are not identical.

External validity is also limited because synthetic graphs and a simple transmission rule cannot reproduce all features of real information diffusion, including repeated interactions, changing behavior, content characteristics, platform algorithms, and individual differences.

Therefore, the findings should be interpreted specifically as results of the defined simulation experiment.

## 14. Reproducibility

The study is implemented in Python using NetworkX, NumPy, Pandas, and Matplotlib.

A fixed random seed of 42 is used, and the notebook records the complete experimental procedure from network construction through simulation, analysis, visualization, export, and validation.

The notebook can therefore be rerun from top to bottom to reproduce the computational workflow.

## 15. Future Work

Several extensions could make the experiment stronger:

1. Test several transmission probabilities instead of only 0.20.
2. Repeat the experiment with different network sizes.
3. Generate multiple graph realizations for each topology.
4. Control edge count more strictly when comparing topologies.
5. Compare additional network measures such as average path length and degree distribution.
6. Measure time to several spread thresholds, such as 25%, 50%, 75%, and 90%.
7. Introduce more realistic transmission rules, such as limited transmission attempts or temporary inactivity.

These extensions would help determine whether the observed advantage of the Scale-Free topology remains stable under broader experimental conditions.

## 16. Conclusion

Under the simulated conditions, network topology affected the **speed** of information spread more clearly than its final extent.

The Scale-Free network reached 50% information spread fastest, with a mean time of **5.93 simulation steps**, compared with **9.00** for Random and **9.20** for Small-World. By step 30, all three networks reached approximately complete spread.

The results therefore support the hypothesis for this specific simulation setup: network structure can change how quickly information moves through a network. However, because the experiment is synthetic and the topology models do not have identical structural properties, the findings should not be generalized directly to real-world social or rumor-spreading behavior.
