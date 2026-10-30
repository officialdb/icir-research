# Master Experiment Register

**Institute for Cognitive Intelligence Research (ICIR)**  
*Permanent Ledger of Hypotheses, Computational Runs, and Empirical Outcomes*

---

## Registry Rules

1. **Unique Monotonic IDs:** Every experiment is assigned an ID of the form `EXP-YYYY-MM-DD-NNN`.
2. **Immutable Entries:** Once an experiment is completed, its record, metrics, and conclusion cannot be rewritten or erased.
3. **Status Taxonomy:**
   - `PLANNED`: Protocol defined; awaiting compute execution.
   - `IN_PROGRESS`: Computational runs underway across random seeds.
   - `COMPLETED`: Runs finished, metrics validated, hypothesis confirmed.
   - `DISCONFIRMED`: Runs finished, metrics contradict hypothesis (negative result preserved).
   - `INCONCLUSIVE`: Execution invalidated by confounding variables or hardware failure.
4. **Linkage:** Every entry links directly to its protocol documentation in `experiments/protocols/` or relevant notes.

---

## Experiment Ledger

| Experiment ID | Title / Hypothesis | Lead Investigator | Domain | Status | Git Commit | Primary Finding |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `EXP-2026-11-01-001` | *Baseline Catastrophic Forgetting Quantification in Multi-Task MLPs and ResNets* | Chilaka Chigozie Micheal | Continual Learning | `PLANNED` | — | Protocol in preparation; benchmarking naive sequential training vs. joint upper bound on Permuted MNIST and Split-CIFAR. |
| `EXP-2026-11-15-002` | *Fisher Information Matrix Approximations under Non-Stationary Shift in Shallow Networks* | Chilaka Chigozie Micheal | Continual Learning | `PLANNED` | — | Evaluating empirical vs. true Fisher regularization under task-incremental boundary shifts. |
| `EXP-2026-12-01-003` | *Dual-Buffer Episodic Replay with Surprise-Weighted Consolidation* | Chilaka Chigozie Micheal | Cognitive Architectures | `PLANNED` | — | Testing hypothesis that prediction error-gated replay reduces memory buffer size while preserving backward transfer. |

---

## Log of Completed Experiments

*(As experimental runs conclude, formal summaries with verified statistical metrics are appended here.)*

*No empirical experiments have reached final completion as of repository establishment (October 2026).*
