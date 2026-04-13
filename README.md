## 1. Problem Description: Capacitated Vehicle Routing Problem (CVRP)

The problem addressed in this project is the **Capacitated Vehicle Routing Problem (CVRP)**, a classic NP-hard combinatorial optimization problem. It is a generalization of the Traveling Salesman Problem (TSP) where multiple vehicles must serve a set of customers from a single depot.

### **The Objective**

The goal is to minimize the **Total Euclidean Distance** traveled by the entire fleet while ensuring that all customers are visited exactly once.

### **Key Constraints**

Based on the **Lecture 5 & 8** principles of constrained optimization, our model enforces:

- **Capacity Constraint:** Every vehicle has a maximum load capacity ($Q = 206$). The sum of demands for all customers on a single route cannot exceed this limit.
- **Flow Conservation:** Each vehicle must return to the depot (Node 1) after completing its assigned route.
- **Single Visit:** Every customer must be visited by exactly one vehicle.
- **Subtour Elimination:** The solution must be a single connected tour per vehicle originating from the depot; isolated "loops" are mathematically prohibited via **MTZ constraints**.

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
| Exact (ILP Subset)  | 1881.87           | 0.01               |
| Heuristic (GA Full) | 35208.47          | 4.02               |
| Optimal Benchmark   | 27591.00          | Historical         |
