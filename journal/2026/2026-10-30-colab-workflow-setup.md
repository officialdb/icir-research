# JRN-2026-10-02: Local-to-Google-Colab Research Workflow & Staging EXP-2026-001

**Date:** 2026-10-30  
**Author:** Chilaka Chigozie Micheal  
**Role:** Founder & Research Lead, ICIR  
**Tags:** `#infrastructure` `#colab` `#experiment-framework` `#EXP-2026-001` `#continual-learning`

---

## 1. Context & Motivation

To execute our foundational 2026–2031 research programme without incurring excessive local hardware costs or maintaining heavyweight on-premise GPU clusters, we established a clean hybrid computational workflow:
- **Local Control Workstation:** All Git version control, protocol definitions, markdown documentation, and notebook source files reside permanently on the local machine (`/home/official/projects/icir-research`).
- **Remote Hardware Acceleration:** Computational operations are executed remotely via an active connection to Google Colab's hosted GPU runtime, providing an NVIDIA Tesla T4 with 14.56 GB dedicated VRAM and PyTorch 2.11.0+cu130.

---

## 2. Infrastructure Design Principles

1. **Strict Dependency Isolation:**  
   Colab's hosted environment comes pre-packaged with PyTorch, CUDA 13.0, and cuDNN. Running arbitrary `pip install` commands inside Colab risks overwriting hardware-optimized binary libraries. Local dependencies (`requirements.txt`) are maintained strictly on the workstation to power VS Code language servers, linting, and local git hooks.
2. **Deterministic Seed Enshrinement:**  
   Per ICIR Empirical Rigor Pillar I, all experiments must run over the canonical five-seed set $\mathcal{S} = [42, 1337, 2026, 7, 99]$.
3. **Transparent Preregistration:**  
   No experiment is executed without a preregistered protocol and immutable ledger entry.

---

## 3. Staging EXP-2026-001: Catastrophic Forgetting Baseline

We formulated and staged [`EXP-2026-001`](../../experiments/protocols/EXP-2026-001-protocol.md) as our inaugural machine learning experiment under Track A (Continual Learning & Dynamic Plasticity).
- **Core Hypothesis ($H_1$):** Naive sequential stochastic gradient descent across 5 orthogonal permuted task manifolds will suffer severe backward catastrophic forgetting ($BWT \le -0.70$), while Joint Offline Training retains high average accuracy ($A_5 \ge 0.90$).
- **Artifacts Created:**
  - Protocol: [`experiments/protocols/EXP-2026-001-protocol.md`](../../experiments/protocols/EXP-2026-001-protocol.md)
  - Notebook: [`experiments/notebooks/EXP-2026-001.ipynb`](../../experiments/notebooks/EXP-2026-001.ipynb)
  - Workflow SOP: [`experiments/WORKFLOW.md`](../../experiments/WORKFLOW.md)
  - Register Entry: [`experiments/register.md`](../../experiments/register.md)

The experiment is staged and ready for formal review and execution on the connected Google Colab GPU runtime.

