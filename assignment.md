# Basics

## Part 1: Basic Concepts

1. What are the three primary criteria we seek when designing an algorithm?
2. List the five basic data structure types and their average time and space complexities.
3. What is the specific objective of the **Interval Scheduling Problem**?
4. List three different types of greedy strategies that could be used for interval scheduling.
5. What is the fundamental difference between an **algorithm** and a **heuristic**?

## Part 2: Application and Analysis

1. Describe the **Earliest Finish Time** greedy algorithm steps.
2. Why is the **Nearest Neighbor** approach for the Traveling Salesman Problem considered
    a heuristic rather than a correct algorithm?
3. Explain the concept of a **Loop Invariant** and its relation to mathematical induction.
4. What does it mean for a problem like the Traveling Salesman Problem (TSP) to be
    **NP-hard**?
5. How does **modularity** contribute to an algorithm’s ease of implementation?

## Part 3: Proof and Complexity

1. **Proof by Contradiction:** Summarize the argument used to prove that the greedy
    algorithm for interval scheduling cannot select fewer jobs than an optimal schedule.
2. **Counterexample Challenge:** Draw or describe a set of intervals where the **Shortest**
    **Interval** greedy strategy fails to provide a maximum-size subset of compatible jobs.
3. **Exhaustive Search Complexity:** Why is exhaustive search (trying n! permutations)
    impractical for a Robot Tour Optimization with 20 points? Provide the approximate
    number of permutations involved.
4. **Algorithm Correctness:** The slides state that "Failure to find a counterexample... does
    not mean the algorithm is correct." Explain why mathematical induction is preferred over
    trial-and-error for proving correctness.
5. **Complexity Trade-offs:** Compare **Quicksort** and **Mergesort** based on their worst-case
    time complexity and space complexity.


## Advanced Algorithms & Data Structures

## Programming Assignment: Interval Scheduling

## Empirical Runtime and Optimality Study

### 1 Objectives

By completing this assignment, students will:

- Implement and compare multiple greedy algorithms for interval scheduling.
- Implement an exhaustive (exponential-time) algorithm to compute the optimal so-
    lution for small instances.
- Design controlled synthetic datasets and understand how input distributions affect
    algorithm behavior.
- Empirically measure running time growth and compare it with theoretical Big-O
    complexity.
- Analyze solution quality versus optimal for different greedy criteria.
- Practice rigorous experimental methodology and result interpretation.

### 2 Problem Definition

You are given a set of n intervals:
I ={(si,fi)}ni=
where siis the start time and fiis the finish time, with si< fi.
Two intervals are compatible if they do not overlap:
fi≤ sj or fj≤ si

Goal: Select a maximum-size subset of pairwise non-overlapping intervals.

### 3 Algorithms to Implement

#### 3.1 Greedy Algorithms (Polynomial Time)

Implement the following greedy strategies. Each algorithm must:

1. Sort intervals by the specified criterion
2. Scan in sorted order and select the next compatible interval


Required greedy criteria

- Earliest Finish Time (EFT): sort by increasing fi
- Earliest Start Time (EST): sort by increasing si
- Shortest Duration (SD): sort by increasing (fi− si)

Each greedy algorithm should return:

- the number of selected intervals
- (optional) the selected set of intervals

Theoretical runtime:
O(n logn)

#### 3.2 Exhaustive Algorithm (Exponential Time)

Implement an algorithm that computes the true optimal solution. This algorithm is required
only for small values of n.
Choose one of the following approaches:

Subset Selection

- Precompute interval compatibility
- Enumerate all subsets and keep the largest feasible one

Worst-case time:
O(n 2 n)

The exhaustive algorithm will be used as an optimality oracle for validating greedy solutions.

### 4 Dataset Generation

#### 4.1 Choosing the Time HorizonT

Let:

- n be the number of intervals
- D be the maximum (or expected) interval duration

To ensure meaningful and comparable experiments, the time horizon T must scale with n.
Default rule:
T = α· n· D
where α > 0 controls the overlap density.
This choice keeps the expected overlap behavior approximately stable as n increases.


#### 4.2 Overlap Regimes

Students must test all three overlap regimes.

```
α Regime Description
α≈ 0 .1 High overlap Dense conflicts
α≈ 1 Medium overlap Balanced conflicts
α≈ 5 Low overlap Sparse conflicts
```
#### 4.3 Required Dataset Types

Uniform Random Dataset

- Choose D (e.g., 10 or 100)
- Set T = α· n· D
- Generate:
    si∼ Uniform[0,T ), di∼ Uniform[1,D], fi= si+ di
- for high-overlap dataset, choose α = 0. 1
- for medium-overlap dataset, choose α = 1
- for low-overlap dataset, choose α = 5

### 5 Experimental Protocol

#### 5.1 Input Sizes

Greedy algorithms:
n∈{ 210 , 211 ,..., 220 }

Exhaustive algorithm:
n∈{ 5 , 10 , 15 ,...,nmax}
where nmaxis the largest size that completes in reasonable time.

#### 5.2 Trials

- Minimum of 10 trials per configuration
- Report mean and standard deviation

#### 5.3 Timing Rules

- Exclude data generation time
- Use high-resolution timers
- Perform at least one warm-up run


### 6 Big-O Validation Methodology

#### 6.1 Greedy Algorithms

Expected complexity:
O(n logn)

Required plots:

- Runtime t(n) vs. n (log–log scale)
- Normalized runtime: t(n)
    n log 2 n

#### 6.2 Exhaustive Algorithm

Expected complexity:
O(n 2 n)

Required plots:

- Runtime t(n) vs. n
- Normalized runtime: t(n)
    n 2 n

Students must explain why pruning may reduce runtime for small n but does not change the
worst-case complexity.

### 7 Expected Outcomes

#### 7.1 Solution Quality

- Earliest Finish Time always matches the optimal solution.
- Earliest Start Time and Shortest Duration:
    - Often match the optimum in low-overlap datasets
    - May be suboptimal in high-overlap datasets

#### 7.2 Runtime Behavior

- All greedy variants scale similarly.
- nt log(n) napproaches a constant.
- Exhaustive runtime grows rapidly and exhibits exponential behavior.


#### 7.3 Effect ofT

- Smaller T leads to higher overlap and larger quality differences.
- Larger T leads to sparse conflicts and similar greedy behavior.
- Incorrect scaling of T leads to misleading conclusions.

### 8 Deliverables

- Source code:
    - dataset generator
    - greedy algorithms
    - exhaustive solver
    - benchmarking
- Report:
    - algorithm descriptions
    - complexity analysis
    - justification of T
    - plots and discussion
- Artifacts:
    - Results
    - plots (PDF or PNG)

### 9 Grading Rubric

```
Component Weight
Correctness & feasibility 30%
Experimental rigor 20%
Big-O validation 25%
Comparative analysis 15%
Code quality 10%
```

