# Benchmark Suite & Evaluation Tasks

**Institute for Cognitive Intelligence Research (ICIR)**  
*Standardized Non-Stationary Environments & Continual Learning Benchmarks*

---

## 1. Overview

To evaluate cognitive architectures and continual learning mechanisms on equitable grounds, ICIR adopts three standardized benchmark paradigms reflecting varying degrees of distribution non-stationarity.

---

## 2. Continual Learning Paradigms

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           Continual Learning Paradigms                       │
├──────────────────────────┬──────────────────────────┬───────────────────────┤
│    Task-Incremental      │    Domain-Incremental    │   Class-Incremental   │
│       (Task-IL)          │       (Domain-IL)        │       (Class-IL)      │
├──────────────────────────┼──────────────────────────┼───────────────────────┤
│ Task ID provided at both │ Task ID not provided;    │ Task ID not provided; │
│ train and test time.     │ input distribution changes│ model must classify   │
│ Separate output heads.   │ but output space static. │ across all classes.   │
│ [Least Difficult]        │ [Intermediate]           │ [Most Realistic/Hard] │
└──────────────────────────┴──────────────────────────┴───────────────────────┘
```

### Paradigm A: Task-Incremental (Task-IL)
- **Setting:** Model is informed of the task identifier $t \in \{1, \dots, T\}$ at both training and inference time.
- **Utility:** Isolates catastrophic forgetting within shared representation layers while using task-specific classification heads.

### Paradigm B: Domain-Incremental (Domain-IL)
- **Setting:** The structure of the problem remains constant, but the input distribution $\mathcal{X}$ undergoes non-stationary transformations (e.g., input rotations, permuted pixels, varying sensor modalities). Task ID is not provided at inference.

### Paradigm C: Class-Incremental (Class-IL)
- **Setting:** The agent must solve a growing multi-class problem sequentially without being told which task subset a test sample originated from.
- **Utility:** The primary test of true plasticity-stability balance in cognitive systems.

---

## 3. Standard Benchmark Datasets

| Benchmark | Tasks ($T$) | Input Dimension | Classes | Description & Primary Test |
| :--- | :--- | :--- | :--- | :--- |
| **Permuted MNIST** | $5 - 20$ | $1 \times 28 \times 28$ | 10 | Random pixel permutations; tests capacity against extreme covariate shifts. |
| **Split-MNIST** | 5 | $1 \times 28 \times 28$ | 2 per task | Consecutive 2-digit classification; standard baseline for class-incremental forgetting. |
| **Split-CIFAR-100** | 10 or 20 | $3 \times 32 \times 32$ | 10 or 5 per task | Complex visual feature drift; evaluates representation degradation in deep backbones. |
| **Continual GridWorld** | Dynamic | Vector state | Discrete actions | Non-stationary reward landscapes; evaluates policy retention in autonomous loops. |

---

## 4. Benchmark Execution Standards

1. **Synthetic Generation:** Where possible, benchmark generators should generate non-stationary streams programmatically to eliminate multi-gigabyte dataset downloads.
2. **Deterministic Task Splits:** All class splits and permutation orders must be deterministically generated using the master seeds defined in [`experiments/README.md`](../experiments/README.md).
3. **Zero Data Leakage:** Validation splits must be strictly segregated; future task data must never be visible to normalizers or data loaders during task $t$.
