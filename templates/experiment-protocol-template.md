# Experiment Protocol & Report Template

**Institute for Cognitive Intelligence Research (ICIR)**  
*Document ID: EXP-TEMPLATE-v1.0*

---

## 1. Experiment Metadata

| Field | Detail |
| :--- | :--- |
| **Experiment ID** | `EXP-YYYY-MM-DD-NNN` (e.g., `EXP-2026-11-05-001`) |
| **Title** | Concise, descriptive title of the empirical inquiry |
| **Investigator(s)** | Name(s) and role(s) |
| **Research Track** | Continual Learning / Cognitive Architectures / Autonomous Intelligence / Resilience |
| **Date Initiated** | YYYY-MM-DD |
| **Date Concluded** | YYYY-MM-DD |
| **Status** | `PROPOSED` \| `IN_PROGRESS` \| `COMPLETED` \| `DISCONFIRMED` \| `INCONCLUSIVE` |
| **Code Commit Hash** | Git SHA (40 chars) corresponding to code state |

---

## 2. Hypothesis & Theoretical Grounding

### Formal Hypothesis
> *State clearly and falsifiably what phenomenon or performance characteristic is being tested.*  
> **Example:** Modulating synaptic plasticity using Fisher Information weights computed over episodic memory buffers reduces backward catastrophic forgetting on Task 1 by $\ge 25\%$ compared to standard experience replay, without increasing memory storage requirements.

### Theoretical Context
- What mathematical principles, neuro-cognitive models, or architectural limitations motivate this inquiry?
- Which past studies or theoretical lemmas are being extended or challenged?

---

## 3. Experimental Setup & Methodology

### 3.1 Task & Non-Stationary Distribution Paradigm
- **Dataset / Environment:** (e.g., Split-CIFAR100, Permuted MNIST, Continual GridWorld, Custom Synthetic Sequence)
- **Shift Paradigm:** Task-Incremental / Domain-Incremental / Class-Incremental
- **Number of Tasks / Phases:** $T = \{1, 2, \dots, N\}$

### 3.2 Models & Baselines
- **Proposed Architecture / Algorithm:** Detailed description of parameters and mechanisms.
- **Baselines for Comparison:**
  - Standard Fine-Tuning (Lower bound / Naive baseline)
  - Joint Training on All Tasks (Upper bound / Offline oracle)
  - State-of-the-art baselines (e.g., EWC, ER, GEM, L2P)

### 3.3 Quantitative Metrics
Specify the mathematical definition of all tracked metrics:
1. **Average Accuracy ($A_T$):** Mean accuracy across all observed tasks at step $T$.
2. **Backward Transfer ($BWT$):**
   $$BWT = \frac{1}{T-1} \sum_{i=1}^{T-1} (R_{T, i} - R_{i, i})$$
   *(Where $R_{j, i}$ is test performance on task $i$ after training on task $j$)*
3. **Forward Transfer ($FWT$):** Evaluation of zero-shot transfer to unseen upcoming distributions.
4. **Memory Footprint & Computational Overhead:** Parameter count growth and FLOPS per sample.

---

## 4. Execution Environment & Reproducibility Parameters

```yaml
environment:
  python_version: "3.10.x"
  torch_version: "2.x.x"
  os: "Linux Ubuntu 24.04 LTS"
  accelerator: "NVIDIA RTX 4090 / CPU"
  deterministic_flags: true

parameters:
  random_seeds: [42, 1337, 2026, 7, 99]
  batch_size: 64
  learning_rate: 0.001
  optimizer: "AdamW"
  weight_decay: 0.01
```

---

## 5. Empirical Results

### 5.1 Primary Performance Summary

| Model / Approach | Avg Accuracy ($A_T$) | Backward Transfer ($BWT$) | Memory Overhead | Status |
| :--- | :--- | :--- | :--- | :--- |
| *Naive Fine-Tuning* | $0.00 \pm 0.00$ | $-0.00$ | $1.0\times$ (None) | Baseline |
| *Baseline 1 (e.g. EWC)* | $0.00 \pm 0.00$ | $-0.00$ | $1.0\times$ | Standard |
| *Proposed Method* | $0.00 \pm 0.00$ | $-0.00$ | $1.1\times$ | Tested |
| *Joint Training (Oracle)* | $0.00 \pm 0.00$ | $0.00$ | N/A (Non-continual) | Upper Bound |

### 5.2 Ablation Study

| Configuration | Removed / Modified Component | Impact on $A_T$ | Impact on $BWT$ |
| :--- | :--- | :--- | :--- |
| `Config A` | Full system | $0.00$ | $0.00$ |
| `Config B` | Without episodic gating | $0.00$ | $0.00$ |
| `Config C` | Without regularized consolidation | $0.00$ | $0.00$ |

---

## 6. Analysis & Observations

### 6.1 Confirmatory Findings
- What parts of the initial hypothesis were supported by the data?

### 6.2 Negative / Disconfirmatory Results & Anomalies
- Where did the proposed method fail or perform worse than expected?
- Were there edge cases where catastrophic forgetting occurred unexpectedly?

### 6.3 Qualitative Behavioral Insights
- Analysis of representations, loss landscapes, or activation patterns.

---

## 7. Conclusions & Next Steps

- **Core Takeaway:** Concise 2–3 sentence synthesis of the findings.
- **Follow-up Inquiries:** What subsequent experiment or theoretical refinement does this result demand?
- **Register Update:** Confirmed logged in [`experiments/register.md`](../experiments/register.md).
