# Continual Learning

**Institute for Cognitive Intelligence Research (ICIR)**  
*Lifelong Learning, Catastrophic Forgetting Mitigation, and Stability-Plasticity Equilibrium*

---

## 1. Domain Scope

**Continual Learning** (also termed lifelong or incremental learning) lies at the core of ICIR’s research mission. While standard machine learning assumes static datasets and batch training, real-world cognitive agents must learn continuously from non-stationary streams of data without forgetting previously acquired skills or suffering from catastrophic interference.

Our inquiries encompass:
- **Catastrophic Forgetting:** Formal characterization of why gradient updates on task $\mathcal{T}_B$ overwrite representations optimized for task $\mathcal{T}_A$.
- **The Stability-Plasticity Dilemma:** Engineering systems that balance sufficient flexibility (plasticity) to acquire new knowledge with sufficient rigidity (stability) to protect historical memory.
- **Dual-Memory Architectures:** Implementing computational analogues of hippocampal-neocortical Complementary Learning Systems (CLS).
- **Continual Representation Learning:** Preserving generalizable latent representations across open-ended task streams.

---

## 2. Taxonomy of Continual Learning Paradigms

```
                                 Continual Learning Taxonomy
                                              │
         ┌────────────────────────────────────┼────────────────────────────────────┐
         │                                    │                                    │
         ▼                                    ▼                                    ▼
┌───────────────────┐                ┌───────────────────┐                ┌───────────────────┐
│ Regularization    │                │ Replay & Memory   │                │ Parameter         │
│ Approaches        │                │ Approaches        │                │ Isolation         │
├───────────────────┤                ├───────────────────┤                ├───────────────────┤
│ Penalize updates  │                │ Store or generate │                │ Allocate dedicated│
│ to parameters     │                │ samples from past │                │ parameter sub-    │
│ critical to prior │                │ tasks; interleave │                │ networks for each │
│ tasks.            │                │ with current data.│                │ task stream.      │
│ (EWC, SI, MAS)    │                │ (ER, DER++, CLS)  │                │ (PackNet, L2P)    │
└───────────────────┘                └───────────────────┘                └───────────────────┘
```

### 2.1 Regularization-Based Methods
These methods augment the task loss with a penalty term that discourages changes to parameters considered critical for earlier tasks:

$$\mathcal{L}_{\text{total}}(\theta) = \mathcal{L}_{\text{current}}(\theta) + \sum_{i} \frac{\lambda}{2} \Omega_i (\theta_i - \theta_i^*)^2$$

Where $\Omega_i$ represents the importance weight of parameter $i$ (e.g., diagonal of the Fisher Information Matrix in EWC, path integral of gradients in Synaptic Intelligence).

### 2.2 Replay & Memory-Based Methods
Inspired by biological memory consolidation, rehearsal methods retain an episodic memory buffer $\mathcal{M}$ containing historical exemplars or employ generative models to synthesize historical distributions, interleaving pseudo-past data into the current gradient update:

$$\mathcal{L}_{\text{replay}}(\theta) = \alpha \mathbb{E}_{(x, y) \sim \mathcal{D}_{\text{new}}} [\ell(f_\theta(x), y)] + (1 - \alpha) \mathbb{E}_{(x_{\text{mem}}, y_{\text{mem}}) \sim \mathcal{M}} [\ell(f_\theta(x_{\text{mem}}), y_{\text{mem}})]$$

### 2.3 Parameter Isolation & Dynamic Architecture
Rather than sharing all parameters across all tasks, isolation methods allocate distinct sub-networks, modular routing adapters, or dynamically expand the capacity of the model, freezing prior modules to mathematically prevent interference.

---

## 3. Mathematical Metrics for Continual Evaluation

To quantify the balance between retention, adaptation, and transfer across $T$ sequential tasks, ICIR mandates four standard metrics:

1. **Average Accuracy ($A_T$):**
   $$A_T = \frac{1}{T} \sum_{j=1}^T R_{T, j}$$
   *(where $R_{i, j}$ is the performance on task $j$ after completing training on task $i$)*

2. **Backward Transfer ($BWT$):**
   $$BWT = \frac{1}{T-1} \sum_{j=1}^{T-1} (R_{T, j} - R_{j, j})$$
   - $BWT < 0$: Catastrophic forgetting has occurred.
   - $BWT = 0$: Perfect retention with zero degradation.
   - $BWT > 0$: Synergistic backward transfer (learning new tasks improved past task mastery).

3. **Forward Transfer ($FWT$):**
   $$FWT = \frac{1}{T-1} \sum_{j=2}^T R_{j-1, j}$$
   Measures zero-shot capacity on upcoming task $j$ before any gradient step on $j$ is taken.

4. **Forgetting Measure ($F_T$):**
   $$F_T = \frac{1}{T-1} \sum_{j=1}^{T-1} \max_{k \in \{1, \dots, T-1\}} (R_{k, j} - R_{T, j})$$

---

## 4. Directory Structure

```
continual-learning/
├── README.md               # This document: domain overview, taxonomy, and metrics
└── notes/                  # Mathematical proofs, algorithmic derivations, and proposals
    └── README.md           # Index of continual learning research notes
```

---

## 5. Active Research Challenges at ICIR

1. **Task-Agnostic Boundary Detection:** Identifying when distribution shift occurs without explicit task labels or environmental oracle signals.
2. **Online Single-Pass Continual Learning:** Learning under extreme streaming constraints where samples are presented exactly once with no mini-batch resampling.
3. **Episodic Compression & Selective Replay:** Designing surprise-driven eviction policies for replay buffers to optimize memory efficiency under bounded storage.
