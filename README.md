# ACIT4610-1-Assignment-2

## How to Run

1. Install Python (>=3.9 recommended).

2. Libraries Used: 
   - vrplib
   - matplotlib
   - numpy
   - random
   - scipy
   - copy
   - time

To install the required libraries, run: pip install vrplib matplotlib numpy scipy

3. To run the NSGA II algorithm, open the NSGA2.ipynb notebook. Here, you can either:
   - Select Run All to execute all cells. 
   - Navigate to "NSGA II - Without Feasibility (Feasibility Below)" section to run NSGA II without feasibility constraints by running the corresponding cells one by one. 
   - Navigate to "Main Algorithm with Feasibility Check" section to run NSGA II with feasibility constraints by running the corresponding cells one by one.

4. To run the VEGA algorithm, open the VEGA.ipynb notebook. Here, you can either:
   -  Select Run All to execute all cells.
   -  Above the "VEGA with feasibility" section, to run the VEGA algorithm with no feasibility constraints, run all corresponding cells one by one.
   -  Under the "VEGA with feasibility" section, to run the VEGA algorithm with feasibility constraints, run all corresponding cells one by one.

## Data / Source
This repository includes CVRPLIB instances from: https://vrp.atd-lab.inf.puc-rio.br/index.php/en/links (Last accessed Oct 9, 2025)

The instance files are stored in the data/ folder.

This repository includes the following CVRPLIB instances:
- "A-n32-k5.vrp": 32 customers, 5 vehicles - Small Instance
- "A-n37-k5.vrp": 37 customers, 5 vehicles - Small Instance
- "B-n78-k10.vrp": 78 customers, 10 vehicles - Medium Instance
- "B-n63-k10.vrp": 63 customers, 10 vehicles - Medium Instance
- "X-n125-k30.vrp":125 customers, 30 vehicles - Large Instance
- "X-n148-k46.vrp": 148 customers, 46 vehicles - Large Instance

The A and B instances were originally proposed by Augerat et al. (1995), and the X instances were introduced by Uchoa et al. (2017).

# Project Overview 
This project implements solutions for the Capacitated Vehicle Routing Problem (CVRP) using Python. 

Objectives:
- Minimize total travel distance across all vehicles.
- Minimize the standard deviation of route lengths to balance the workload among vehicles.

Individual Representation:
- Each individual is represented as a list: [route, vehicles assigned]
- Route: A permutation of customer locations (indices)
- Vehicles Assigned: Integer list, of the same length as "route". The element at index "i" specifies which vehicle is assigned to the customer at "route[i]"

Genetic Operators: Crossover & Mutation

Crossover: Combines two individual parents to produce offspring (Route Crossover and Vehicles Crossover)
- Route Crossover: Partially Mapped Crossover and
- Vehicles Crossover: Uniform Crossover

Mutation: Introduces random variation within an individual (Route Mutation or Vehicles Mutation)
- Route Mutation: Scramble Mutation 
- Vehicles Mutation: Random Resetting

## NSGA-II
The "NSGA-2.ipynb" notebook contains an implementation of the NSGA-II algorithm to solve CVRP instances and optimize multiple objectives.

The "NSGA-2.ipynb" notebook is organized into multiple parts:
1. Developing functions and testing them
2. NSGA-II algorithm without vehicle capacity constraints
3. NSGA-II algorithm with vehicle capacity constraints

### Feasibility 
Feasibility ensures that the load of each vehicle in one individual do not exceed capacity.
- After randomly generating individuals, if they are not feasible, a vehicle repair function tries to repair them for a number of iterations
- After generating offspring, if they are not feasible, the same repair function tries to repair them for a number of iterations
- Some individuals may remain infeasible when repair fails. In this case, the dominance method was modified following Deb et al. (2002) (doi: 10.1109/4235.996017)
#### Dominance with Constraints.
There are three possible cases:
1. One solution feasible, and the other is not -> Feasible solution dominates
2. Both solutions NOT feasible -> Solution that has a smaller constraint violation dominates
3. Both are feasible: Use standard NSGA-II dominance. solution1 dominates Solution 2 when:
    - It is not worse than solution2 in all objectives
    - It is better than solution2 in at least one objective
## VEGA
The "VEGA.ipynb" notebook contains an implementation of the NSGA-II algorithm to solve CVRP instances and optimize multiple objectives.

The "NSGA-2.ipynb" notebook is organized into multiple parts:
2. NSGA-II algorithm without vehicle capacity constraints
3. NSGA-II algorithm with vehicle capacity constraints

### Feasibility 
Feasibility ensures that the load of each vehicle in one individual do not exceed capacity.
- After randomly generating individuals, if they are not feasible, a vehicle repair function tries to repair them for a number of iterations
- After generating offspring, if they are not feasible, the same repair function tries to repair them for a number of iterations
- Some individuals may remain infeasible when repair fails. In this case, the evaluation method was modified by applying penalties for infeasible solutions. The degree of penalty depends on the amount of constraint violation, so solutions that violate the constraint more are penalized more heavily, and on penalty factors. Multiple penalty factors were tested, and the notebook includes the final values.

# Experimental Setup
Test three instance sizes: small, medium, large; with two instances for each. 

For each instance size, there are three parameter scenarios.
Small:
- Scenario 1: Population Size = 50, Number of Generations = 50, Crossover Rate = 0.9, Mutation Rate = 0.1
- Scenario 2: Population Size = 50, Number of Generations = 50, Crossover Rate = 0.7, Mutation Rate = 0.1
- Scenario 3: Population Size = 50, Number of Generations = 50, Crossover Rate = 0.9, Mutation Rate = 0.3

Medium:
- Scenario 1: Population Size = 70, Number of Generations = 70, Crossover Rate = 0.9, Mutation Rate = 0.1
- Scenario 2: Population Size = 50, Number of Generations = 70, Crossover Rate = 0.9, Mutation Rate = 0.1
- Scenario 3: Population Size = 70, Number of Generations = 50, Crossover Rate = 0.9, Mutation Rate = 0.1

Large:
- Scenario 1: Population Size = 90, Number of Generations = 100, Crossover Rate = 0.9, Mutation Rate = 0.1
- Scenario 2: Population Size = 100, Number of Generations = 100, Crossover Rate = 0.9, Mutation Rate = 0.1
- Scenario 3: Population Size = 90, Number of Generations = 100, Crossover Rate = 0.9, Mutation Rate = 0.01

Results are reported for 20 runs.


# References
K. Deb, A. Pratap, S. Agarwal and T. Meyarivan, "A fast and elitist multiobjective genetic algorithm: NSGA-II," in IEEE Transactions on Evolutionary Computation, vol. 6, no. 2, pp. 182-197, April 2002, doi: 10.1109/4235.996017.

