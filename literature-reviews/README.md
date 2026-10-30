# Literature Reviews & Systematic Surveys

**Institute for Cognitive Intelligence Research (ICIR)**  
*Systematic Literature Surveys, Paper Critiques, and Thematic Taxonomies*

---

## 1. Domain Scope

Rigorous research begins with a comprehensive, unvarnished appraisal of prior art. The **Literature Reviews** division maintains structured surveys, critical deconstructions of seminal and frontier literature, and canonical reading lists across ICIR’s four research tracks.

All paper reviews are authored using [`templates/literature-review-template.md`](../templates/literature-review-template.md).

---

## 2. Canonical Reading Lists

### 2.1 Track A: Continual Learning & Catastrophic Forgetting
- **Kirkpatrick et al. (2017):** *Overcoming catastrophic forgetting in neural networks.* PNAS. (Introduced Elastic Weight Consolidation via Fisher Information).
- **Lopez-Paz & Ranzato (2017):** *Gradient Episodic Memory for Continual Learning.* NeurIPS. (Formalized $BWT$ and $FWT$ metrics).
- **Buzzega et al. (2020):** *Dark Experience Replay: Simple and Efficient Continual Learning with Replay.* NeurIPS.
- **van de Ven et al. (2022):** *Three types of incremental learning.* Nature Machine Intelligence.
- **De Lange et al. (2021):** *A continual learning survey: Defying forgetting in statistical learning.* IEEE TPAMI.

### 2.2 Track B: Cognitive Architectures & Neuro-Symbolic Systems
- **Anderson, J. R. (2007):** *How Can the Human Mind Occur in the Physical Universe?* Oxford University Press. (The foundational ACT-R monograph).
- **Laird, J. E. (2012):** *The Soar Cognitive Architecture.* MIT Press.
- **Baars, B. J. (1988):** *A Cognitive Theory of Consciousness.* Cambridge University Press. (Global Workspace Theory).
- **Dehaene, Lau, & Kouider (2017):** *What is consciousness, and could machines have it?* Science.
- **Lake, Ullman, Tenenbaum, & Gershman (2017):** *Building machines that learn and think like people.* Behavioral and Brain Sciences.
- **Bengio, Y. (2017):** *The Consciousness Prior.* arXiv preprint.

### 2.3 Track C: Autonomous Agency & Active Inference
- **Friston, K. (2010):** *The free-energy principle: a unified brain theory?* Nature Reviews Neuroscience.
- **Parr, Pezzulo, & Friston (2022):** *Active Inference: The Free Energy Principle in Mind, Brain, and Behavior.* MIT Press.
- **Amodei et al. (2016):** *Concrete Problems in AI Safety.* arXiv preprint.

### 2.4 Track D: Software Systems & Cybersecurity in Autonomous AI
- **Saltzer & Schroeder (1975):** *The Protection of Information in Computer Systems.* Proceedings of the IEEE. (Foundational security design principles).
- **Greshake et al. (2023):** *Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection.* ACM CCS.

---

## 3. Directory Structure

```
literature-reviews/
├── README.md               # This document: reading lists, review methodology, and taxonomies
└── reviews/                # In-depth structured critiques of individual papers and monographs
    └── README.md           # Index of completed and in-progress literature reviews
```

---

## 4. Review Methodology & Review Standards

Every review conducted under ICIR must explicitly answer:
1. What is the fundamental theoretical bottleneck identified?
2. Did the authors rely on hidden assumptions (e.g., task oracle availability, stationary feature backbones)?
3. How reproducible are the empirical baselines?
4. What can ICIR extract or build upon in our ongoing roadmap?
