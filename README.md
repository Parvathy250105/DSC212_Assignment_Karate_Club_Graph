# Community Detection in Zachary's Karate Club using Spectral Modularity
---
#### - Parvathy Suresh Menon
#### - IMS24165
---

## 1. Project Overview

This project implements a community detection algorithm on the famous **Zachary's Karate Club graph**. The core of the assignment is to use an iterative **spectral modularity bisection** method to recursively split the graph into its most cohesive communities.

A key part of the analysis is not just the final partition, but **tracking the evolution of node-level centrality metrics** (Degree, Betweenness, Closeness, and Clustering) at each step of the split. This allows us to observe how a node's "importance" or "role" in the network changes as its local community is refined.

This project is a practical exercise in network analysis, linear algebra (eigenvectors), and data visualization.

---

## 2. Tasks Completed & Key Features

- [x] **Modularity Matrix Implementation:** Calculates the global modularity matrix ($B$) for the graph.
- [x] **Recursive Spectral Bisection:** Implements the core algorithm. It iteratively splits communities based on the sign of the leading eigenvector of the *refined* subgraph modularity matrix ($B^{(C)}$).
- [x] **Per-Iteration Metric Tracking:** At each step of the bisection, the code calculates and stores four centrality metrics (Degree, Betweenness, Closeness, Clustering) for every node *within its current subgraph*.
- [x] **Community Split Visualization:** Generates a series of "time-lapse" plots showing the graph at each iteration. These plots use a fixed layout (`pos`), node labels, and unique colors for each community to clearly show the splits.
- [x] **Metric Evolution Plotting:** Generates four line graphs, one for each centrality metric, showing how the value for each node changes across the iterations.
- [x] **Final Summary & Analysis:** The notebook concludes with a summary table of the final communities (including their size and average metrics) and a written discussion analyzing the results.

---

## 3. Algorithm & Citation

The community detection method used in this notebook is based on the **spectral modularity maximization** algorithm, most prominently described by M. E. J. Newman.

The algorithm uses the **modularity matrix ($B$)**, and its core idea is that the leading eigenvector of this matrix can be used to optimally bisect a network into two communities. This process is then applied recursively to the sub-communities.

* **Core Algorithm:** Newman, M. E. J. (2006). Modularity and community structure in networks. *Proceedings of the National Academy of Sciences*, 103(23), 8577-8582. (This paper formalizes the use of the modularity matrix's eigenvectors for community detection).
* **Dataset:** Zachary, W. W. (1977). An information flow model for conflict and fission in small groups. *Journal of Anthropological Research*, 33(4), 452-473. (This is the original paper describing the Karate Club social network).



---

## 4. How the Algorithm Works

The community detection algorithm is implemented in the `find_communities` function. It uses a queue-based approach to manage the communities that still need to be tested for splits.

1.  **Initialize:** The `queue` (a "to-do" list) is initialized with the entire graph as the first community.
2.  **Iterate:** The `while queue:` loop continues as long as there are communities in the "to-do" list.
3.  **Calculate Metrics:** At the start of each loop, the function calculates and saves the metrics for the *current* state of all partitions (both "to-do" and "final").
4.  **Test Community:** The function pulls the next community `C` from the `queue`.
5.  **Refine Matrix:** It calculates the refined modularity matrix $B^{(C)}$ for this specific subgraph, as described by Newman.
6.  **Find Eigenpair:** It finds the leading eigenvalue (`lambda_1`) and eigenvector (`u_1`) of $B^{(C)}$.
7.  **Decision to Split:**
    * If `lambda_1 <= 0`, the community is indivisible. It is moved to the `final_communities` list.
    * If `lambda_1 > 0`, a split will improve modularity. The community is split into `C_plus` and `C_minus` based on the sign of the values in the `u_1` eigenvector.
8.  **Validate Split:** A check is performed to ensure the split was non-trivial (i.e., both `C_plus` and `C_minus` are non-empty). If the split is valid, the two new, smaller communities are added back to the `queue` to be tested. If the split is trivial, the original `C` is considered final.
9.  **Complete:** The loop terminates when the `queue` is empty. The function returns the complete history of all partitions and all metrics.

---


## 5. Summary of Results & Conclusion

The algorithm successfully partitioned the graph into **[5]** final communities after **[4]** successful splits.

* **Key Finding 1 (The Splits):** The **first split** was the most significant, cleanly separating the two main factions of the club (those loyal to "Mr. Hi" Node 0, and those to the "Officer" Node 33). The subsequent splits identified smaller, more tight-knit sub-cliques within these two main groups, and also isolated **Node 11** as an outlier.

* **Key Finding 2 (The Metrics):** The metric evolution plots provided the richest insights. The **Betweenness Centrality** plot was the most telling: the two leaders (0 and 33) started with very high betweenness, which **plummeted to zero** after the first split. This demonstrates their role change: they went from being "global brokers" (connecting two factions) to "local leaders" (at the center of their own isolated community).

**Conclusion:** A node's role and "importance" in a network are not static. They are dynamic properties entirely dependent on the community structure being analyzed. This algorithm effectively demonstrated this by showing how node centralities change as the graph is broken down from a global network into its local, cohesive parts.
