# Literature Review & Paper Deconstruction Template

**Institute for Cognitive Intelligence Research (ICIR)**  
*Document ID: LIT-TEMPLATE-v1.0*

---

## 1. Bibliographic Metadata

| Field | Details |
| :--- | :--- |
| **Paper Title** | *Exact Paper Title* |
| **Authors** | Author 1, Author 2, et al. |
| **Year / Venue** | YYYY (e.g., NeurIPS, ICLR, ICML, Cognitive Science, arXiv) |
| **DOI / URL** | `https://doi.org/...` or `https://arxiv.org/abs/...` |
| **Review Date** | YYYY-MM-DD |
| **Reviewer** | Reviewer Name |
| **Thematic Tags** | `#continual-learning` `#cognitive-architecture` `#memory-systems` `#active-inference` |

```bibtex
@inproceedings{citekey,
  author    = {Author, A. and Coauthor, B.},
  title     = {Title of the Work},
  booktitle = {Proceedings of the Conference},
  year      = {YYYY}
}
```

---

## 2. Executive Synthesis

Provide a concise 3–4 sentence distillation answering:
1. What exact bottleneck does this work attempt to resolve?
2. What is the fundamental theoretical or architectural premise?
3. What is the primary empirical conclusion?

---

## 3. Problem Formulation & Theoretical Foundations

- **Formal Setting:** How is the task mathematically formalized? (e.g., sequence of Markov Decision Processes, stream of non-stationary distributions $\mathcal{D}_1, \dots, \mathcal{D}_T$).
- **Core Assumptions:** What unstated or explicit assumptions does the paper rely upon? (e.g., clear task boundaries, infinite compute, stationary feature extractors).

---

## 4. Key Mechanisms & Architectural Innovation

```
[Input Representation] ──► [Proposed Mechanism / Filter] ──► [Consolidated State]
```

- **Algorithm / Architecture Description:** Detail the mechanics, mathematical loss functions, and state updates.
- **Novelty vs Prior Art:** What differentiates this method from predecessor techniques?

---

## 5. Critical Evaluation & Methodological Critique

### Strengths
- What makes this approach theoretically sound or empirically compelling?
- What evaluation choices were commendable?

### Limitations & Fragilities
- Are the benchmarks artificial or prone to data leakage?
- How does the method scale as the number of tasks $T \to \infty$?
- Does it require knowing task identity at test time (Task-IL vs Class-IL)?
- What is the computational and memory complexity trade-off?

---

## 6. Relevance to ICIR Research Programme

| ICIR Research Track | Specific Intersection & Application |
| :--- | :--- |
| **Track A: Continual Learning** | *How does this inform our plasticity-stability work?* |
| **Track B: Cognitive Architectures** | *Can this serve as a component in our dual-process design?* |
| **Track C: Autonomous Intelligence** | *Does this support active inference or goal stability?* |
| **Track D: Cyber-Resilience** | *Does this introduce novel attack vectors (e.g., memory corruption)?* |

---

## 7. Actionable Follow-Up Inquiries

1. **Reproduction Target:** Is this a candidate for experimental verification in `experiments/`?
2. **Open Research Question:** What did the authors leave unanswered that ICIR can investigate?
3. **Cross-References:** Which papers in `literature-reviews/` does this build on or contradict?

