# Experiment Output Artifacts: EXP-2026-001

**Institute for Cognitive Intelligence Research (ICIR)**  
*Experiment ID:* `EXP-2026-001`  
*Related Protocol:* [`experiments/protocols/EXP-2026-001-protocol.md`](../../protocols/EXP-2026-001-protocol.md)  
*Related Notebook:* [`experiments/notebooks/EXP-2026-001.ipynb`](../../notebooks/EXP-2026-001.ipynb)

---

## Directory Organization

```
experiments/outputs/EXP-2026-001/
├── README.md               # Artifact ledger and storage documentation
├── metrics/                # Structured numerical outputs and evaluation records
│   ├── metrics_schema.json # JSON Schema for verification
│   └── EXP-2026-001_metrics.json (Generated post-execution)
├── figures/                # High-resolution visualization artifacts (PNG, 300 DPI)
│   └── EXP-2026-001_retention_curves.png (Generated post-execution)
└── logs/                   # Execution console logs and runtime diagnostic outputs
```

---

## Artifact Retention Policy

1. **Lightweight Artifacts (`metrics/`, `figures/`):** Committed directly to Git as persistent empirical records.
2. **Heavy Tensor Weights:** Raw model checkpoints (`.pt`, `.pth`) are excluded by `.gitignore` in compliance with ICIR Minimalist Software Engineering principles.
3. **Execution Status:** Staged and ready for execution on connected Google Colab GPU runtime.

