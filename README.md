

# **Project Description — Recursive Spectral Modularity Partitioning of Zachary’s Karate Club Network**

This project develops and analyzes a recursive spectral community-detection framework applied to **Zachary’s Karate Club**, a widely used benchmark graph in social network analysis. The aim is to identify hierarchical community structure by repeatedly applying a **modularity-based spectral bipartitioning** procedure, while simultaneously tracking how network properties evolve across successive splits.

## **Overview**

The method begins by constructing the **modularity matrix** for a given subset of nodes, using global degree information and the total number of edges in the original graph. The **leading eigenvector** of this matrix encodes the dominant direction of modular structure. When the corresponding eigenvalue is positive, the signs of the eigenvector entries provide a principled way to divide the subset into two candidate communities. This idea extends the classical Newman–Girvan modularity-optimization approach to a recursive setting.

Starting from the full vertex set, the algorithm repeats this procedure on each newly created subset, subject to constraints on **minimum community size** and **maximum recursion depth** to prevent over-fragmentation. Each partition operation is logged, including the parent set, the resulting two child sets, and the leading eigenvalue that justified the split.

## **Objectives**

The project is designed to:

1. **Reveal hierarchical community structure** in the Karate Club network using data-driven spectral signals.
2. **Document each partition step** through a clear partition history, enabling interpretability and comparison.
3. **Analyze structural changes** by computing local metrics—degree, betweenness, closeness, and clustering—for each community at every stage.
4. **Visualize the entire process**, including graph partitions and the temporal evolution of centrality measures for selected nodes.

## **Algorithmic Foundations**

1. **Modularity Matrix Construction:**
   For a subset of nodes, the modularity matrix ( B = A - \frac{kk^T}{2m} ) is formed using degrees from the original graph.

2. **Spectral Bipartitioning:**
   The **leading eigenpair** ((\lambda_1, v_1)) is computed using either sparse eigensolvers or dense fallback methods. A split is accepted only when ( \lambda_1 > 0 ).

3. **Recursive Partitioning:**
   The sign pattern of (v_1) yields two communities. The procedure is recursively applied to each, maintaining depth and size constraints.

4. **Metric Tracking:**
   After each partition, centrality measures are recomputed within each community, producing a sequence of metric dictionaries for later visualization.

5. **Visualization:**
   The evolving partitions are plotted with consistent node layouts, and metric trajectories are visualized for all nodes, with optional highlighting of influential individuals (e.g., nodes 0 and 33).

## **Software Architecture**

The implementation is modular and organized across several components:

* **`config.py`** — global experimental parameters (community size constraints, verbosity, recursion depth, figure settings, random seed).
* **`graph_utils.py`** — loader for the Karate Club graph (NetworkX).
* **`modularity.py`** — modularity matrix construction and robust leading-eigenpair computation.
* **`partitioning.py`** — spectral bipartitioning logic and recursive community-detection engine.
* **`metrics.py`** — computation of intra-community centrality and clustering metrics.
* **`visualization.py`** — high-quality plotting of graph partitions and temporal metric evolution.
* **`main.py`** — orchestrates the full experiment, produces all figures, and prints final summaries.

This architecture ensures clarity, testability, and extensibility.

## **Outputs and Interpretation**

The system produces:

* **Partition snapshots** (PNG), showing community evolution at each iteration.
* **Metric-evolution plots** for degree, betweenness, closeness, and clustering.
* **Final community assignments** based on all accepted recursive splits.
* **Partition logs** summarizing parent subset sizes, resulting split sizes, and corresponding leading eigenvalues.

These outputs reveal not only the final community structure but also *how* the structure emerges as modularity-guided splits progress. The metric trajectories help identify pivotal nodes (leaders, brokers, bridges) and quantify their changing influence as the network is decomposed.

## **Reproducibility and Control**

A fixed (`RANDOM_SEED`) ensures deterministic layout and execution. Adjustable limits such as **`MIN_COMMUNITY_SIZE`** and **`MAX_RECURSION_DEPTH`** mitigate over-segmentation and guarantee algorithmic stability