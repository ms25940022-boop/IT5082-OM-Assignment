## 1. Problem Description: Capacitated Vehicle Routing Problem (CVRP)

The problem addressed in this project is the **Capacitated Vehicle Routing Problem (CVRP)**, a classic NP-hard combinatorial optimization problem. It is a generalization of the Traveling Salesman Problem (TSP) where multiple vehicles must serve a set of customers from a single depot.

### **Objective Function**

The objective is to minimize the total Euclidean distance traveled by all vehicles:

$$\min Z = \sum_{i \in V} \sum_{j \in V} d_{ij} x_{ij}$$

Where $d_{ij}$ is the Euclidean distance between node $i$ and node $j$.

### **Decision Variable**

$$x_{ij} = \begin{cases} 1, & \text{if a vehicle travels from node } i \text{ to node } j \\ 0, & \text{otherwise} \end{cases}$$

### **Main Constraints**

1.  **Customer Visit Constraint:** Each customer must be visited by exactly one vehicle.
    $$\sum_{i \in V, i \neq j} x_{ij} = 1, \quad \forall j \in \{2, \dots, n\}$$
2.  **Flow Conservation Constraint:** If a vehicle arrives at a customer node, it must also leave that node.
    $$\sum_{i \in V, i \neq j} x_{ij} = \sum_{k \in V, k \neq j} x_{jk}, \quad \forall j \in V$$
3.  **Capacity Constraint:** The total load carried by a vehicle must not exceed its capacity ($Q = 206$).
4.  **MTZ Constraint (Subtour Elimination):** Prevents the formation of subtours (small isolated loops) that do not include the depot.
    $$u_i - u_j + q_j \le Q(1 - x_{ij}), \quad \forall i, j \in \{2, \dots, n\}, i \neq j$$
    *Where $u_i$ is the cumulative load at node $i$.*

## 2. Dataset Analysis

The project utilizes the **X-n101-k25** instance from the Uchoa et al. (2017) benchmark suite.

| Feature                 | Specification                                    |
| :---------------------- | :----------------------------------------------- |
| **Number of Nodes**     | 101 (1 Depot + 100 Customers)                    |
| **Vehicle Fleet**       | 25 Vehicles                                      |
| **Vehicle Capacity**    | 206 Units                                        |
| **Distance Metric**     | Euclidean ($EUC\_2D$)                            |
| **Demand Distribution** | Non-uniform (specified in `data/X-n101-k25.vrp`) |

## 3. Methodology Overview

We compare two distinct optimization paradigms:

1.  **Exact Method (Integer Linear Programming):**
    - **Focus:** Global Optimality (Lecture 5).
    - **Approach:** Formulated using the Miller-Tucker-Zemlin (MTZ) method.
    - **Tool:** `PuLP` library with the CBC solver.

2.  **Heuristic Method (Genetic Algorithm):**
    - **Focus:** Computational Scalability (Lecture 8).
    - **Approach:** Population-based search using **Ordered Crossover (OX)** to maintain permutation validity and **Elitism** to preserve the best-found routes.

## 4. Individual Contributions

- **Student Name (MS25941562):** Implemented the **Exact Method (ILP)** using the PuLP library and MTZ subtour elimination constraints. Focus: Mathematical accuracy and global optimality for small subsets.
- **Student Name (MS25940022):** Implemented the **Heuristic Method (Genetic Algorithm)**. Focus: Scalability for large-scale logistics and metaheuristic search strategies.

## 5. Setup Instructions

1. Create virtual environment: `python -m venv .env`
2. Activate: `.env\Scripts\activate` (Windows) or `source .env/bin/activate` (Mac)
3. Install dependencies: `pip install -r requirements.txt`
4. Register kernel: `python -m ipykernel install --user --name=opti --display-name "OptiEnv"`

## 6. How to Run

1. Ensure the dataset is located in `./data/X-n101-k25.vrp`.
2. Run `MS25941562_ILP.ipynb` for the Exact formulation.
3. Run `MS25940022_GA.ipynb` for the Heuristic results and visualizations.

## 7. Results Summary

| Method              | Result (Distance) | Execution Time (s) |
| :------------------ | :---------------- | :----------------- |
| Exact (ILP Subset)  | 7540.41           | 299.9              |
| Heuristic (GA Full) | 32402             | 5.1                |
| Optimal Benchmark   | 27591             | Historical         |

### **Methodological Comparison**

| Feature              | ILP                             | GA                         |
| :------------------- | :------------------------------ | :------------------------- |
| **Problem Scale**    | Small (21 Nodes)                | Large (101 Nodes)          |
| **Solution Quality** | Global Optimum (Perfect)        | Near-Optimal (Good enough) |
| **Execution Time**   | 299.9 Seconds (for 21 nodes)    | Seconds (for 101 nodes)    |
| **Complexity**       | Exponential ($$\mathcal{O}(N^2)$$)| Polynomial/Linearized      |
