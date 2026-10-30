# Contributing Guidelines & Research Integrity Protocol

**Institute for Cognitive Intelligence Research (ICIR)**  
*Open Collaboration & Peer Review Framework*

---

## Welcome to ICIR

Thank you for your interest in contributing to the **Institute for Cognitive Intelligence Research (ICIR)**. We welcome independent researchers, software engineers, cognitive scientists, and cybersecurity specialists who wish to contribute to the theoretical and empirical foundations of cognitive architectures, continual learning, and autonomous intelligence.

Because ICIR is dedicated to high-standard open scientific inquiry, all contributions must adhere to the methodological, stylistic, and ethical principles outlined below.

---

## Code of Conduct & Scientific Integrity

1. **Academic Honesty:** Plagiarism, fabricated experimental metrics, cherry-picked baselines, and hallucinated citations will not be tolerated. All references must point to verifiable publications, preprints, or primary repositories.
2. **Independent Status Representation:** Never describe ICIR or its contributors as holding state-chartered university accreditation or institutional endorsements that do not exist. Transparency is paramount.
3. **Constructive Critique:** Scientific debate must remain focused on theoretical validity, mathematical correctness, empirical methodology, and code quality. Ad hominem or exclusionary behavior is unacceptable.

---

## Types of Contributions

We actively welcome contributions in several structured formats:

1. **Theoretical Notes (`*/notes/`):** Mathematical proofs, formal definitions, and conceptual synthesis in LaTeX/Markdown.
2. **Literature Reviews (`literature-reviews/reviews/`):** Systematic decompositions of published papers, taxonomy extensions, and comparative analyses using [`literature-review-template.md`](templates/literature-review-template.md).
3. **Experimental Suites (`experiments/`):** Implementation of benchmark tasks, ablation studies, or reproduction attempts logged in [`experiments/register.md`](experiments/register.md).
4. **Code Implementations:** Minimal, highly readable implementations of continual learning algorithms, memory buffers, or cognitive dispatch loops.
5. **Corrections & Errata:** Identifying flaws in mathematical derivations or non-reproducible empirical claims.

---

## Workflow & Git Standards

### 1. Branch Naming Conventions
Create a descriptive branch off `main`:
- `theory/<topic>`: Theoretical derivations, notes, mathematical proofs.
- `lit/<paper-or-topic>`: Literature review or reading list update.
- `exp/<experiment-id>`: Empirical experiment, benchmark script, or results update.
- `arch/<module-name>`: Implementation of a cognitive architecture module.
- `fix/<issue-description>`: Bug fix, errata correction, or documentation adjustment.

### 2. Commit Message Structure
We follow conventional commits with research-oriented prefixes:
```text
<type>(<scope>): <concise description in imperative mood>

[optional body providing theoretical or empirical rationale]
[optional issue reference or experiment ID]
```
Allowed types:
- `theory`: Mathematical foundations, proofs, conceptual notes.
- `exp`: Experimental scripts, benchmark execution, register updates.
- `lit`: Literature reviews and bibliographic citations.
- `feat`: New software component or architectural module.
- `fix`: Bug fix in code or correction in mathematical derivation.
- `docs`: General repository documentation updates.
- `refactor`: Code reorganization without algorithmic change.

---

## Empirical Reproducibility Checklist

Every Pull Request introducing an experiment or computational benchmark must satisfy the following checklist before merge:

- [ ] **Deterministic Seeding:** Explicit random seeds for Python `random`, NumPy, and PyTorch (e.g., `seed = 42`).
- [ ] **Dependency Minimization:** No transient, heavy, or undocumented libraries. Standard PyTorch, NumPy, and standard library are preferred.
- [ ] **Hardware & Environment Specification:** Compute requirements (GPU/CPU, memory, OS) documented in the experiment protocol.
- [ ] **Negative Results Disclosed:** If an architectural modification performed worse than the baseline, the result is fully reported, not concealed.
- [ ] **Experiment Register Updated:** The experiment is logged in [`experiments/register.md`](experiments/register.md) with date, ID, hypothesis, and outcome.

---

## Python & Code Style Guidelines

1. **Formatting & Style:** Follow PEP 8 guidelines. Write clean, idiomatic Python (Python 3.10+).
2. **Type Annotations:** Use explicit type hints for public function signatures.
3. **Tensor Shapes:** Document tensor dimensions in comments when writing non-trivial tensor transformations:
   ```python
   def forward(self, memory_bank: torch.Tensor, query: torch.Tensor) -> torch.Tensor:
       # memory_bank: [batch_size, num_slots, feature_dim]
       # query: [batch_size, query_dim]
       ...
   ```
4. **Self-Contained Executables:** Keep benchmark and experiment scripts executable standalone with clear CLI arguments (using standard `argparse`).

---

## Markdown & Mathematics Formatting

1. **LaTeX Math:** Multi-term formulas must use standard LaTeX blocks:
   $$L_{\text{continual}}(\theta) = L_{\text{task}}(\theta) + \sum_{i} \frac{\Omega_i}{2} (\theta_i - \theta_i^*)^2$$
   Inline mathematical variables should be wrapped in single dollar signs (`$x \in \mathbb{R}^d$`).
2. **Heading Hierarchy:** Maintain clean Markdown hierarchy (`#`, `##`, `###`).
3. **Links:** Prefer relative repository links for internal references.

---

## Questions and Research Discussion

To propose an inquiry, open an issue labeled `discussion` or submit a research proposal draft using the templates provided in the `templates/` directory.
