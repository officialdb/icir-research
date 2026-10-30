# Deep Learning

**Institute for Cognitive Intelligence Research (ICIR)**  
*Representation Learning, Neural Dynamics, and Parameterized Function Approximators*

---

## 1. Domain Scope

Deep neural networks represent the primary substrate for modern artificial perception and representation learning. Within ICIR, deep learning is investigated not as an end in itself, but as a high-capacity, differentiable representation manifold whose dynamical properties must be harnessed, stabilized, and integrated into broader cognitive architectures.

Our research covers:
- **Representation Manifolds:** Geometric properties of latent embeddings, disentanglement, and feature reuse across tasks.
- **Attention & Sequence Mechanics:** Transformer self-attention, linear state-space models (SSMs), and associative memory retrieval.
- **Overparameterization & Neural Tangent Kernels (NTK):** Implicit regularization, double descent, and flat vs. sharp minima in non-convex loss surfaces.
- **Energy-Based & Generative Representations:** Energy-Based Models (EBMs), Variational Autoencoders (VAEs), and predictive coding backbones.

---

## 2. Core Theoretical Dimensions

### 2.1 The Representation Drift Phenomenon
When deep neural networks parameterized by weights $\theta = \{\mathbf{W}_1, \dots, \mathbf{W}_L\}$ are updated via stochastic gradient descent on a novel task, the lower-layer representations (feature extractors) undergo geometric rotations:

$$\Delta \mathbf{z}_l = \sigma(\mathbf{W}_l^{(t+1)} \mathbf{z}_{l-1}) - \sigma(\mathbf{W}_l^{(t)} \mathbf{z}_{l-1})$$

Even subtle rotations in early layers cascade non-linearly through higher layers, causing the decision boundaries of prior tasks to collapse catastrophically. Understanding the stability of internal layer representations is central to resolving catastrophic forgetting.

### 2.2 Attention Mechanics as Dynamic Associative Memories
The scaled dot-product attention mechanism:

$$\text{Attention}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = \text{softmax}\left(\frac{\mathbf{Q}\mathbf{K}^\top}{\sqrt{d_k}}\right)\mathbf{V}$$

can be mathematically interpreted as an addressable associative memory system where queries dynamically route information from keys to values. ICIR explores how attention structures can serve as the differentiable substrate for cognitive working memory.

---

## 3. Directory Structure

```
deep-learning/
├── README.md               # This document: domain overview and theoretical foundations
└── notes/                  # Architecture dissections, scaling notes, and layer analyses
    └── README.md           # Index of deep learning research notes
```

---

## 4. Key Research Directions

1. **Latent Space Plasticity:** Quantifying how the geometry of latent representations changes under continuous fine-tuning vs. isolated low-rank updates (e.g., LoRA, adapter modules).
2. **State-Space Models for Persistent Context:** Evaluating the memory retention characteristics of Structured State-Space Models (e.g., S4, Mamba) over prolonged sequential tasks compared to quadratic attention.
