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
├── report.ipynb                  # Main Jupyter notebook: complete analysis, code, and results
├── report.html                   # Exported HTML version for browser viewing
├── report.pdf                    # PDF export of notebook
├── .git/                         # Git repository for version control
└── README.md                     # This file
```

**Primary Submission:**
- **report.ipynb** — Jupyter notebook containing all theory, code, experiments, and analysis:
  - **Cells 1-4**: Theory sections (Parts 1-3) answering all 15 questions with proofs and counterexamples
  - **Cells 5-13**: Algorithm implementations (EFT, EST, SD greedy + exhaustive optimal solver)
  - **Cell 14**: Benchmarking and timing framework intro with T formula justification
  - **Cell 15**: Benchmarking code with rigorous timing protocol (warm-up + measurement phases)
  - **Cell 16**: Big-O validation intro with pruning explanation
  - **Cells 17-18**: Greedy and exhaustive complexity plots (3 overlap regimes each)
  - **Cell 19**: Complexity comparison tables
  - **Cell 20**: Solution quality analysis with boxplots
  - **Cells 21+**: Comparative analysis and conclusions

**Supporting Files:**
- **report.html** — Viewable in any web browser without Jupyter installation
- **report.pdf** — Printable PDF version for archival

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
- **Cells 1-4**: Complete theoretical explanations (Parts 1, 2, 3) with proofs and counterexamples
- **Cells 5-13**: Implementation of all algorithms (EFT, EST, SD, Exhaustive + helpers)
- **Cell 14**: Benchmarking framework intro with T = α·n·D justification
- **Cell 15**: Benchmarking code with complete timing protocol
  - 1 warm-up execution (discarded)
  - 10 measurement runs per trial
  - 10 independent trials per configuration
- **Cell 16**: Big-O validation section explaining why pruning reduces average-case runtime but not worst-case complexity
- **Cells 17-18**: Greedy and exhaustive complexity plots
  - Log-log scale and normalized runtime for all 3 overlap regimes
- **Cell 19**: Complexity comparison tables across all overlap regimes
- **Cell 20**: Solution quality analysis with boxplots
- **Cells 21+**: Comparative analysis and conclusions

### Run Experiments
To reproduce all experiments from scratch:

```bash
jupyter notebook report.ipynb
```

Then:
1. Select **Kernel → Restart Kernel and Clear All Outputs** to start fresh
2. Run all cells sequentially (Shift+Enter)
3. Observe results printed to console and plots generated inline
4. Benchmarking will automatically display mean/std tables for each configuration

**What happens when cells run:**
1. **Cells 1-13**: Theory, helper functions, and algorithms (instant)
2. **Cell 14**: Benchmarking framework setup (instant)
3. **Cell 15**: Greedy and exhaustive benchmarking with result tables (~20-60 minutes)
4. **Cell 16**: Big-O validation explanation (instant)
5. **Cells 17-18**: Generate complexity plots across all overlap regimes (~1 minute)
6. **Cell 19**: Complexity comparison tables (instant)
7. **Cell 20**: Solution quality analysis with boxplots (~2-5 minutes)
8. **Cells 21+**: Summary tables and conclusions (instant)

### Expected Runtime

**Benchmarking Statistics:**
- **Greedy algorithms**: 11 input sizes × 3 overlap regimes × 10 trials × 10 measurements = 3,300 executions
  - Typical runtime on standard laptop: 5–15 minutes (highly hardware-dependent)
- **Exhaustive algorithm**: 4 input sizes × 3 overlap regimes × 10 trials × 10 measurements = 1,200 executions
  - Typical runtime on standard laptop: 10–40 minutes (exponential growth dominates; highly hardware-dependent)
  - For $n=20$: ~20 million subsets checked per trial
- **Solution quality analysis**: 6 input sizes × 3 overlap regimes × 10 trials × 3 algorithms = 540 executions
  - Typical runtime: 2–5 minutes

**Total estimated time**: 20–60 minutes (depending on CPU speed, background processes, and OS overhead)

Hardware impact example:
- Modern CPU (Intel i7/i9, M3 Pro or better): Lower end of range
- Older/slower CPU or running other applications: Upper end of range
- First run may be slower due to Jupyter kernel initialization

## Experimental Protocol

### Input Sizes
- **Greedy algorithms**: $n \in \{2^{10}, 2^{11}, \ldots, 2^{20}\}$ = {1024, 2048, 4096, 8192, 16384, 32768, 65536, 131072, 262144, 524288, 1048576}
  - 11 input sizes, minimum 10 independent trials per size
- **Exhaustive algorithm**: $n \in \{5, 10, 15, 20\}$
  - 4 input sizes, minimum 10 independent trials per size per overlap regime
  - Limited to small $n$ due to exponential time complexity: $n=20$ requires ~20 million subset evaluations

### Dataset Generation
**Time Horizon Formula**: $T = \alpha \cdot n \cdot D$

**Parameters:**
- $D = 10$ (maximum interval duration)
- Number of trials per configuration: 10
- Random seed: `np.random.seed(42)` (for reproducibility)

**Overlap Regimes:**
- **High Overlap**: $\alpha = 0.1$ (dense conflicts, limited selection room)
- **Medium Overlap**: $\alpha = 1.0$ (balanced conflicts)
- **Low Overlap**: $\alpha = 5.0$ (sparse conflicts, many selection options)

**Interval Properties:**
- Start time: $s_i \sim \text{Uniform}[0, T)$
- Duration: $d_i \sim \text{Uniform}[1, D]$
- Finish time: $f_i = s_i + d_i$

### Timing Protocol
- **Data generation**: EXCLUDED from timing measurements
- **Timer**: `time.perf_counter()` (high-resolution monotonic clock)
- **Warm-up phase**: 1 execution per trial (results discarded to stabilize CPU cache)
- **Measurement phase**: 10 executions per trial (averaged within trial)
- **Aggregation**: Mean and standard deviation computed across 10 independent trials per (n, α) configuration
- **Statistical rigor**: Each reported value = average of 10 measurement runs on a unique dataset, with std dev capturing variability across trials

### Algorithm Implementations

| Algorithm | Time Complexity | Space Complexity | Correctness |
|---|---|---|---|
| EFT | $O(n \log n)$ | $O(1)$ extra | Optimal ✓ |
| EST | $O(n \log n)$ | $O(1)$ extra | Heuristic |
| SD | $O(n \log n)$ | $O(1)$ extra | Heuristic |
| Exhaustive | $O(n \cdot 2^n)$ | $O(n)$ recursion stack | Optimal ✓ |

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
