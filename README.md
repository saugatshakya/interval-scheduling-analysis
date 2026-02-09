# Interval Scheduling: Empirical Runtime and Optimality Study

## Project Overview

This repository corresponds exactly to the implementation and experiments discussed in the submitted report.

This project implements and empirically evaluates multiple algorithms for the **Interval Scheduling Problem**:

- **Greedy Algorithms**: Earliest Finish Time (EFT, optimal), Earliest Start Time (EST, heuristic), Shortest Duration (SD, heuristic)
- **Exhaustive Algorithm**: Optimal solution via subset enumeration for small instances
- **Objective**: Compare solution quality and validate theoretical Big-O complexity through empirical measurement

## Key Features

- ✅ Controlled synthetic dataset generation with configurable overlap regimes
- ✅ Benchmarking framework with statistical analysis (mean, std dev across trials)
- ✅ Empirical complexity validation plots (log-log, normalized runtime)
- ✅ Solution quality analysis across different input distributions
- ✅ Complete reproducibility with publication-ready visualizations

## Repository Contents

```
assignment_1/
├── report.ipynb                  # Main submission: complete analysis with code and results
├── report.html                   # Exported HTML version of report
└── README.md                     # This file
```

**Key Files:**
- **report.ipynb** — Primary submission document with theory, code, results, and conclusions

## Installation & Setup

**Requirements:**
- Python 3.8+
- Jupyter Notebook or JupyterLab
- NumPy
- Pandas
- Matplotlib
- Seaborn (for boxplot visualizations in quality analysis)
- SciPy

**Install dependencies:**
```bash
pip install jupyter numpy pandas matplotlib seaborn scipy
```

## Quick Start

### View the Report
Open `report.ipynb` in Jupyter to see:
- Complete theoretical explanations and proofs
- All algorithm implementations (greedy and exhaustive)
- Benchmarking code and empirical runtime measurements
- Complexity validation plots (log-log, normalized runtime)
- Solution quality analysis across overlap regimes
- Detailed findings and conclusions

### Run Experiments
To reproduce all experiments and generate plots:

```bash
jupyter notebook report.ipynb
```

Then run all cells sequentially. This will:
1. Generate controlled synthetic datasets for greedy and exhaustive algorithms
2. Measure empirical runtime across multiple input sizes
3. Generate complexity validation plots
4. Analyze solution quality across overlap regimes
5. Display results as tables and visualizations

### Expected Runtime
- **Greedy algorithms** ($n = 2^{10}$ to $2^{20}$): Typical runtime on a standard laptop is ~10 minutes
- **Exhaustive algorithm** ($n = 5$ to $20$): Typical runtime on a standard laptop is ~30 minutes
- **Total**: ~40 minutes (hardware-dependent; may vary significantly on older or slower machines)

## Experimental Protocol

### Input Sizes
- **Greedy algorithms**: $n \in \{2^{10}, 2^{11}, \ldots, 2^{20}\}$, at least 10 trials per size
- **Exhaustive algorithm**: $n \in \{5, 10, 15, 18, 19, 20\}$, 3–5 trials per size (time-limited)

### Dataset Generation
Time horizon: $T = \alpha \cdot n \cdot D$

**Overlap Regimes:**
- High overlap: $\alpha = 0.1$ (dense conflicts)
- Medium overlap: $\alpha = 1.0$ (balanced)
- Low overlap: $\alpha = 5.0$ (sparse)

**Interval Properties:**
- Start time: $s_i \sim \text{Uniform}[0, T)$
- Duration: $d_i \sim \text{Uniform}[1, D]$
- Finish time: $f_i = s_i + d_i$

### Algorithm Complexities

| Algorithm | Time Complexity | Space Complexity | Correctness |
|---|---|---|---|
| EFT | $O(n \log n)$ | $O(1)$ extra (excluding sorting and input storage) | Optimal |
| EST | $O(n \log n)$ | $O(1)$ extra (excluding sorting and input storage) | Heuristic |
| SD | $O(n \log n)$ | $O(1)$ extra (excluding sorting and input storage) | Heuristic |
| Exhaustive | $O(n \cdot 2^n)$ | $O(n)$ for recursion stack | Optimal |

## Key Results

### Finding 1: EFT is Always Optimal
The Earliest Finish Time greedy algorithm consistently matches the optimal solution across all overlap density regimes, confirming its optimality.

### Finding 2: EST and SD Can Fail
Under high-overlap conditions, the EST and SD heuristics can produce suboptimal solutions, with quality gaps up to 30% in some instances.

### Finding 3: Empirical Complexity Matches Theory
- Greedy runtime scales as $\Theta(n \log n)$, validated by normalized runtime plots
- Exhaustive runtime grows exponentially as $\Theta(2^n)$; several years of runtime for exhaustive interval scheduling at $n = 20$, illustrating exponential growth

## Reproducibility

All experiments are fully reproducible from the Jupyter notebook. Results are deterministic given the random seed (`np.random.seed(42)`). 

To reproduce:

```bash
# Install dependencies
pip install jupyter numpy pandas matplotlib seaborn scipy

# Open notebook
jupyter notebook report.ipynb

# Run all cells
```

All tables and plots will be generated automatically.

## Troubleshooting

### Experiments run slowly?
- First execution takes longer as dependencies compile
- On older hardware, may take significantly longer than estimated
- Each algorithm cell can be run independently if needed

### Plots not displaying?
- Ensure you're using Jupyter Notebook or JupyterLab
- Matplotlib should render plots automatically
- If not, check that your Jupyter environment has display capabilities enabled

### Results differ slightly between runs?
- Expected due to OS-level timing variations
- Random seed (`np.random.seed(42)`) ensures data generation is identical
- Statistical variation is captured by mean and standard deviation in results tables

## Author
Saugat Shakya

## Academic Integrity
This project is submitted as course coursework for an Algorithms class. For questions about reuse or academic policies, please contact your instructor.
