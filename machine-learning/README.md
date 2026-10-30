# Machine Learning

**Institute for Cognitive Intelligence Research (ICIR)**  
*Statistical Learning Theory, Inductive Biases, and Optimization Dynamics*

---

## 1. Domain Scope

While modern artificial intelligence frequently equates machine learning with deep neural networks, foundational statistical learning theory provides the indispensable vocabulary for understanding why models generalize, how inductive biases operate, and where optimization algorithms break down when data distributions drift.

The **Machine Learning** division at ICIR analyzes:
- **Statistical Learning Foundations:** Empirical Risk Minimization (ERM), Structural Risk Minimization (SRM), and generalization bounds.
- **Inductive Biases:** How architectural priors constrain the functional hypothesis space $\mathcal{H}$ to enable learning from finite samples.
- **Optimization Trajectories:** Gradient dynamics, learning rate schedules, momentum mechanics, and convergence behaviors in non-convex settings.
- **Online & Streaming Learning:** Regret bounds, sequential prediction, and kernel methods under streaming observation constraints.

---

## 2. Core Concepts & Theoretical Tensions

### 2.1 The Breakdown of Empirical Risk Minimization Under Drift
Classical statistical learning theory assumes independent and identically distributed (i.i.d.) observations drawn from a stationary distribution $P(X, Y)$:

$$\mathcal{R}_{\text{emp}}(f) = \frac{1}{N} \sum_{i=1}^N \ell(f(x_i), y_i) \xrightarrow[N \to \infty]{\text{a.s.}} \mathcal{R}_{\text{true}}(f) = \mathbb{E}_{(X,Y) \sim P}[\ell(f(X), Y)]$$

In autonomous agents and continual settings, the data-generating distribution changes over time: $P_t(X, Y) \neq P_{t+1}(X, Y)$. ERM applied naively to the current stream $P_t$ minimizes current empirical risk while completely neglecting historical expected risk $\mathcal{R}_{\text{true}}^{(1:t-1)}(f)$, mathematically guaranteeing catastrophic forgetting.

### 2.2 Online Convex Optimization & Regret Minimization
In online decision scenarios, an agent selects hypotheses $w_t \in \mathcal{W}$ sequentially against a sequence of convex loss functions $f_1, f_2, \dots, f_T$. Performance is evaluated via **external regret**:

$$\text{Regret}_T = \sum_{t=1}^T f_t(w_t) - \min_{u \in \mathcal{W}} \sum_{t=1}^T f_t(u)$$

Sublinear regret ($\text{Regret}_T / T \to 0$ as $T \to \infty$) guarantees that the agent performs as well as the best fixed hypothesis in hindsight—a crucial theoretical baseline for evaluating continual adaptation strategies.

---

## 3. Directory Structure

```
machine-learning/
├── README.md               # This document: theoretical scope and core concepts
└── notes/                  # Analytical monographs and methodological comparisons
    └── README.md           # Index of classical learning notes
```

---

## 4. Active Inquiries in Classical Learning

1. **Non-Stationary Regret Bounds:** Extending Follow-the-Regularized-Leader (FTRL) algorithms to piece-wise stationary environments with discrete distribution shifts.
2. **Inductive Bias Quantifiers:** Formulating metric spaces to measure the geometric alignment between a model's inductive bias and task structures in continual reinforcement settings.
