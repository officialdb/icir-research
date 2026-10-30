# Experiment Protocol & Report: EXP-2026-001

**Institute for Cognitive Intelligence Research (ICIR)**  
*Document ID: EXP-2026-001-v1.0*  
*Permanent Identifier:* `EXP-2026-001`  
*Related Notebook:* [`experiments/notebooks/EXP-2026-001.ipynb`](../notebooks/EXP-2026-001.ipynb)

---

## 1. Experiment Metadata

| Field | Detail |
| :--- | :--- |
| **Experiment ID** | `EXP-2026-001` (Canonical Ledger ID: `EXP-2026-11-01-001`) |
| **Title** | Empirical Quantification of Catastrophic Forgetting and Stability-Plasticity Dilemma in Sequential Neural Networks |
| **Investigator(s)** | Chilaka Chigozie Micheal (Research Lead & Founder, ICIR) |
| **Research Track** | Track A: Continual Learning & Dynamic Plasticity (ICIR 2026–2031 Horizon, Phase 1) |
| **Date Initiated** | 2026-10-30 |
| **Date Concluded** | *Pending Review & Compute Execution* |
| **Status** | `PLANNED` / `STAGED_FOR_REVIEW` |
| **Target Runtime** | Google Colab Hosted GPU Runtime (Tesla T4, 14.56 GB VRAM, PyTorch 2.11.0+cu130, CUDA 13.0) via local VS Code |
| **Code Commit Hash** | `275dfd3` (Staged on branch `main`) |

---

## 2. Hypothesis & Theoretical Grounding

### 2.1 Research Question
> *To what quantitative degree does unconstrained sequential stochastic gradient descent induce catastrophic interference across sequentially presented orthogonal task distributions, and how wide is the empirical stability gap separating naive sequential adaptation from joint offline empirical risk minimization?*

### 2.2 Formal Hypotheses

- **Primary Alternative Hypothesis ($H_1$):**  
  In a 5-task sequential Permuted MNIST benchmark evaluated across the five canonical ICIR pseudo-random seeds ($\mathcal{S} = \{42, 1337, 2026, 7, 99\}$), a multi-layer perceptron trained via unregularized sequential gradient descent without rehearsal will experience catastrophic forgetting on Task 1 exceeding $70\%$ relative accuracy degradation, yielding a terminal Backward Transfer score:
  $$BWT_{\text{naive}} \le -0.70$$
  In contrast, Joint Offline Training (the empirical upper bound oracle) presented with all task distributions simultaneously in an i.i.d. shuffle will retain mean accuracy:
  $$A_{5, \text{joint}} \ge 0.90 \quad \text{with} \quad BWT_{\text{joint}} \ge -0.05$$

- **Null Hypothesis ($H_0$):**  
  Sequential stochastic gradient descent retains prior task representations within standard variance tolerances ($BWT_{\text{naive}} > -0.15$), indicating that catastrophic forgetting does not manifest as a dominant failure mode in the parameterized multi-layer architecture under this task sequence.

### 2.3 Mathematical Foundations

#### The Stability-Plasticity Dilemma & Non-Stationary Empirical Risk
Consider a sequence of $T$ learning tasks $\mathcal{T} = \{1, 2, \dots, T\}$, where each task $t$ is characterized by a distinct data-generating distribution $\mathcal{D}_t$ over input-target space $\mathcal{X} \times \mathcal{Y}$. The true risk for task $t$ under parameterized hypothesis $f(\cdot; \theta)$ with loss function $\ell$ is:
$$\mathcal{R}_t(\theta) = \mathbb{E}_{(x, y) \sim \mathcal{D}_t} \left[ \ell(f(x; \theta), y) \right]$$

In standard offline (joint) empirical risk minimization (ERM), the optimization objective simultaneously minimizes the average risk over all tasks:
$$\theta_{\text{joint}}^* = \arg\min_\theta \frac{1}{T} \sum_{t=1}^T \mathcal{R}_t(\theta)$$

In the continual sequential paradigm, the learner encounters tasks one by one without access to prior distributions:
$$\theta_t^* = \arg\min_\theta \mathcal{R}_t(\theta_{t-1}^*)$$

#### Gradient Interference and Geometric Misalignment
When updating parameters $\theta$ along the gradient flow of task $t$, the parameter adjustment vector is:
$$\Delta \theta_t \propto -\nabla_\theta \mathcal{L}_t(\theta)$$

The directional derivative of the prior task loss $\mathcal{L}_k$ ($k < t$) along this trajectory is:
$$\frac{d}{d\alpha} \mathcal{L}_k(\theta - \alpha \nabla_\theta \mathcal{L}_t(\theta)) \Big|_{\alpha=0} = -\langle \nabla_\theta \mathcal{L}_k(\theta), \nabla_\theta \mathcal{L}_t(\theta) \rangle$$

Whenever the inner product of task gradients is negative:
$$\langle \nabla_\theta \mathcal{L}_k(\theta), \nabla_\theta \mathcal{L}_t(\theta) \rangle < 0$$
any gradient step that decreases error on task $t$ strictly increases the loss on prior task $k$, precipitating catastrophic erasure of previously encoded synaptic weights.

---

## 3. Experimental Setup & Methodology

### 3.1 Task Paradigm: Permuted MNIST (Domain-IL)
- **Dataset:** Standard MNIST handwritten digits ($28 \times 28$ grayscale pixels, 10 output classes, 60,000 train / 10,000 test).
- **Task Construction:** A sequence of $T = 5$ distinct tasks is synthesized. Task 1 corresponds to the canonical MNIST pixel ordering ($\pi_1 = \text{Id}$). For each subsequent task $t \in \{2, 3, 4, 5\}$, an orthogonal fixed pseudo-random permutation $\pi_t \in \mathcal{S}_{784}$ is applied across all input pixels:
  $$x^{(t)} = \pi_t(x), \quad y^{(t)} = y$$
- **Paradigm Classification:** Domain-Incremental Learning (Domain-IL). The output semantic category space remains $\{0, 1, \dots, 9\}$, while the covariate distribution $P(X)$ is severely disrupted by $\pi_t$.

### 3.2 Architectural Specification
- **Topology:** Multi-Layer Perceptron (MLP)
  - Input Layer: $784$ units (flattened $28 \times 28$)
  - Hidden Layer 1: $256$ units + ReLU + Dropout ($p = 0.1$)
  - Hidden Layer 2: $128$ units + ReLU + Dropout ($p = 0.1$)
  - Output Head: $10$ linear units (logits)
- **Total Trainable Parameters:**
  $$(784 \times 256 + 256) + (256 \times 128 + 128) + (128 \times 10 + 10) = 200,960 + 32,896 + 1,290 = 235,146 \text{ parameters}$$

### 3.3 Comparative Regimes
1. **Regime A — Naive Sequential Fine-Tuning (Empirical Lower Bound):**
   The network is trained consecutively on tasks $1 \to 2 \to 3 \to 4 \to 5$. At the transition $t \to t+1$, no prior data is replayed and no synaptic regularization is enforced.
2. **Regime B — Joint Offline Training (Empirical Upper Bound / Oracle):**
   The network is trained on a stationary union of all $T = 5$ tasks presented in an i.i.d. shuffled batch stream for an equivalent total compute budget.

### 3.4 Quantitative Metrics Formulation
Let $R \in \mathbb{R}^{T \times T}$ denote the evaluation matrix, where $R_{i, j} \in [0, 1]$ represents the classification accuracy on the test set of task $j$ after completing training on task $i$.

1. **Terminal Average Accuracy ($A_T$):**
   $$A_T = \frac{1}{T} \sum_{j=1}^T R_{T, j}$$
2. **Backward Transfer ($BWT$):**
   $$BWT = \frac{1}{T-1} \sum_{j=1}^{T-1} (R_{T, j} - R_{j, j})$$
   A value $BWT < 0$ measures the exact magnitude of catastrophic forgetting; $BWT \approx 0$ indicates perfect retention.
3. **Forward Transfer ($FWT$):**
   $$FWT = \frac{1}{T-1} \sum_{j=2}^T R_{j-1, j}$$
   Measures zero-shot transfer on upcoming task distributions prior to training.
4. **Computational Efficiency ($\kappa$):**
   Recorded as wall-clock execution time (seconds) and throughput (samples/second) on the designated hardware runtime.

---

## 4. Execution Environment & Reproducibility Parameters

```yaml
environment:
  platform: "Google Colab Hosted GPU Runtime via VS Code Local Interface"
  accelerator: "NVIDIA Tesla T4 (Driver Version: 535.x+, CUDA: 13.0)"
  gpu_memory: "14.56 GB"
  python_version: "3.11.x (Local) / 3.10-3.11 (Colab)"
  torch_version: "2.11.0+cu130"
  cudnn_deterministic: true
  cudnn_benchmark: false

parameters:
  canonical_seeds: [42, 1337, 2026, 7, 99]
  num_tasks: 5
  batch_size: 128
  learning_rate: 0.001
  weight_decay: 0.0001
  optimizer: "AdamW"
  epochs_per_task: 3
  loss_function: "CrossEntropyLoss"
```

---

## 5. Empirical Results

### 5.1 Expected Outcomes (Theoretical Projections)

| Regime / Model | Expected $A_5$ | Expected $BWT$ | Expected $FWT$ | Memory Buffer | Status |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Naive Sequential (Lower Bound)** | $0.20 \pm 0.05$ | $\le -0.70$ | $0.10 \pm 0.02$ | $0$ (None) | *Theoretical Expectation* |
| **Joint Offline (Upper Bound Oracle)** | $\ge 0.90 \pm 0.02$ | $\ge -0.05$ | N/A | Full dataset | *Theoretical Expectation* |

### 5.2 Actual Experimental Findings

> [!IMPORTANT]
> **Status: PENDING REVIEW & EXECUTION.**  
> In compliance with ICIR protocol, empirical results are never fabricated or simulated prior to actual compute execution on the designated runtime.  
> Following peer review of [`experiments/notebooks/EXP-2026-001.ipynb`](../notebooks/EXP-2026-001.ipynb), the user will trigger execution on the connected Google Colab Tesla T4 GPU runtime. Upon conclusion, verified metric arrays, statistical means, standard deviations, and log references will be populated into the ledger below.

| Regime / Model | Mean $A_5$ ($\pm \sigma$) | Mean $BWT$ ($\pm \sigma$) | Mean $FWT$ ($\pm \sigma$) | Wall Time (s) | Verified Status |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Naive Sequential (Lower Bound)** | *Pending Run* | *Pending Run* | *Pending Run* | *Pending Run* | `PLANNED` |
| **Joint Offline (Upper Bound Oracle)** | *Pending Run* | *Pending Run* | *Pending Run* | *Pending Run* | `PLANNED` |

---

## 6. Analysis & Observations (To be completed post-execution)

- **6.1 Confirmatory / Disconfirmatory Evidence:** Does $BWT_{\text{naive}}$ violate or uphold $H_1$?
- **6.2 Retention Degradation Rate:** Plotting accuracy decay of Task 1 as $t \to 2, 3, 4, 5$.
- **6.3 Hardware Acceleration Profile:** Tesla T4 tensor core utilization and speedup over CPU baseline.

---

## 7. Conclusions & Next Steps

1. **Protocol Verification:** Notebook staged at [`experiments/notebooks/EXP-2026-001.ipynb`](../notebooks/EXP-2026-001.ipynb).
2. **Compute Execution:** Triggered through VS Code connected to Google Colab GPU runtime.
3. **Artifact Logging:** Outputs will automatically persist to `experiments/outputs/EXP-2026-001/metrics/` and `figures/`.
4. **Register Update:** Master entry tracked in [`experiments/register.md`](../experiments/register.md).

