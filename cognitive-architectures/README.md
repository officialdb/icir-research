# Cognitive Architectures

**Institute for Cognitive Intelligence Research (ICIR)**  
*Unified Models of Cognition, Dual-Process Reasoning, and Neuro-Symbolic Synthesis*

---

## 1. Domain Scope

A **Cognitive Architecture** provides a unified computational specification of the invariant structures, memory stores, control mechanisms, and representational transformations that constitute an intelligent mind. While machine learning typically focuses on isolated algorithmic capabilities (e.g., image classification or next-token prediction), cognitive architecture research asks: *How do perception, memory, deliberation, and motor action cohere into an integrated, self-governing cognitive system?*

At ICIR, we investigate the modern synthesis of classical cognitive frameworks with differentiable representation learning:

```
                            ┌─────────────────────────────────────────┐
                            │          Metacognitive Executive        │
                            │      Confidence Monitoring & Routing    │
                            └────────────────────┬────────────────────┘
                                                 │
                   ┌─────────────────────────────┴─────────────────────────────┐
                   │                                                           │
                   ▼                                                           ▼
     ┌───────────────────────────┐                               ┌───────────────────────────┐
     │      System 1 (Neural)    │                               │     System 2 (Symbolic)   │
     │  - Fast Associative Net   │                               │  - Deliberative Planning  │
     │  - Implicit Representations│◄─────────────────────────────►│  - Explicit Verification │
     │  - Continuous Latent Space│                               │  - Discrete Logic/Search  │
     └─────────────┬─────────────┘                               └─────────────┬─────────────┘
                   │                                                           │
                   └─────────────────────────────┬─────────────────────────────┘
                                                 │
                                                 ▼
                                    ┌─────────────────────────┐
                                    │     Global Workspace    │
                                    │   Information Routing   │
                                    │     & Working Memory    │
                                    └─────────────────────────┘
```

---

## 2. Foundational Classical Frameworks & Modern Equivalents

### 2.1 ACT-R (Adaptive Control of Thought — Rational)
- **Classical Formulation:** Developed by John R. Anderson; structures cognition into discrete sensory, motor, declarative (facts/chunks), and procedural (production rules) modules communicating exclusively through bounded buffers.
- **Modern ICIR Interpretation:** Translating procedural production rules into differentiable router mechanisms while modeling declarative chunks via addressable key-value embeddings with temporal decay.

### 2.2 Global Workspace Theory (GWT)
- **Classical Formulation:** Formulated by Bernard Baars and Stanislas Dehaene; posits a working memory capacity bottleneck where specialized, decentralized non-conscious modules compete for access to a global broadcast workspace.
- **Modern ICIR Interpretation:** Utilizing bottlenecked attention channels (e.g., Perceiver-style cross-attention or shared latents) to broadcast critical contextual representations across downstream specialized sub-agents.

### 2.3 Dual-Process Theory (System 1 vs System 2)
- **Classical Formulation:** Distinguishes between System 1 (fast, automatic, unconscious, heuristic) and System 2 (slow, deliberative, rule-following, computationally expensive).
- **Modern ICIR Interpretation:** Gating generative neural inference (System 1) through formal constraint verifiers, Monte Carlo tree search, and proof checkers (System 2) governed by metacognitive uncertainty metrics.

---

## 3. Directory Structure

```
cognitive-architectures/
├── README.md               # This document: domain architecture and theoretical mappings
└── notes/                  # Architecture blueprints, working memory designs, and specs
    └── README.md           # Index of cognitive architecture notes
```

---

## 4. Key Architectural Inquiries at ICIR

1. **Differentiable Working Memory Constraints:** How does bounding working memory capacity force an artificial agent to perform optimal conceptual compression?
2. **Episodic-to-Semantic Transfer Dynamics:** Formalizing the architectural pipeline through which raw episodic memory traces are distilled into structured semantic knowledge graphs.
3. **Metacognitive Gating Protocols:** Designing mathematical thresholding functions that determine when an agent should halt intuitive generation and initiate deliberative search.

