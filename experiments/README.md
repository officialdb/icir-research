# ICIR Experimental Framework & Testbed

**Institute for Cognitive Intelligence Research (ICIR)**  
*Empirical Methodology, Reproducibility Standards, and Benchmark Suite*

---

## 1. Overview

Empirical investigation at ICIR is governed by strict scientific reproducibility standards. All computational experiments must be preregistered in the [Master Experiment Register](register.md) before or immediately upon initiation, accompanied by a completed protocol following [`templates/experiment-protocol-template.md`](../templates/experiment-protocol-template.md).

---

## 2. Directory Structure

```
experiments/
├── README.md               # This document: protocols, rules, and hardware guidelines
├── register.md             # Immutable tabular ledger of all experiments
├── benchmarks/             # Standard task definitions and dataset generators
│   └── README.md           # Benchmark catalog (Split-CIFAR, Permuted MNIST, GridWorld, etc.)
└── protocols/              # Documented protocols and empirical reports (EXP-*)
```

---

## 3. The Five Pillars of ICIR Empirical Rigor

### I. Deterministic Seeding Protocol
All experiments must be evaluated over a standardized set of five distinct pseudo-random seeds:
```python
SEEDS = [42, 1337, 2026, 7, 99]
```
For each seed, all libraries must be explicitly initialized:
```python
import random, os, numpy as np, torch

def seed_everything(seed: int = 42) -> None:
    random.seed(seed)
    os.environ['PYTHONHASHSEED'] = str(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False
```

### II. Evaluation Metrics Standardization
To avoid ambiguous claims, continual learning and cognitive experiments must evaluate:
- **Average Accuracy ($A_T$):** Overall performance across all encountered tasks at terminal step $T$.
- **Backward Transfer ($BWT$):** Quantifying retention versus forgetting:
  $$BWT = \frac{1}{T-1} \sum_{i=1}^{T-1} (R_{T, i} - R_{i, i})$$
  *(Negative values indicate catastrophic forgetting; positive values indicate retroactive facilitation).*
- **Forward Transfer ($FWT$):** Zero-shot transfer to task $k$ before training on task $k$.
- **Memory Overhead ($\Omega$):** Additional memory buffer or parameter expansion required relative to a static baseline.
- **Computational Efficiency ($\kappa$):** Flops and wall-clock time per sample update.

### III. Baseline Parity
Every proposed novel mechanism must be benchmarked against:
1. *Lower Bound:* Naive Sequential Fine-Tuning (no forgetting mitigation).
2. *Upper Bound:* Joint Offline Training (all tasks presented simultaneously in stationary i.i.d. shuffle).
3. *Established Standard Baselines:* EWC (regularization), Rehearsal / Experience Replay (memory), and Dynamic Architecture (isolation).

### IV. Mandatory Disclosure of Disconfirmatory Results
If an experimental hypothesis fails or results are inferior to established baselines, the experiment must still be finalized and marked as `DISCONFIRMED` in [`register.md`](register.md). Disconfirmatory data provides essential scientific insight into the boundary constraints of cognitive mechanisms.

### V. Version Control & Git Pinning
The exact git commit hash representing the codebase state at execution must be recorded in the experiment register and protocol sheet.
