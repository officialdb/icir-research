# ICIR Local-to-Google-Colab Research Workflow & Git Synchronization

**Institute for Cognitive Intelligence Research (ICIR)**  
*Standard Operating Procedure (SOP): Computational Experiment Lifecycle*  
*Target Environment: VS Code Desktop ↔ Remote Google Colab Hosted GPU Runtime*

---

## 1. Architectural Model

The ICIR experimental framework operates on a **decoupled local-client / remote-compute architecture**:

```
┌────────────────────────────────────────────────────────────────────────┐
│                        LOCAL WORKSTATION (Linux)                       │
│                                                                        │
│  Repository: /home/official/projects/icir-research                     │
│  ├── experiments/notebooks/EXP-*.ipynb   <-- Source of truth (.ipynb)  │
│  ├── experiments/protocols/EXP-*.md      <-- Formal scientific record  │
│  ├── experiments/outputs/EXP-*/          <-- Structured metrics/plots  │
│  └── .git/                               <-- Full local Git tracking   │
│                                                                        │
│  VS Code Desktop IDE                                                   │
│  └── Jupyter / Colab Extension Protocol                                │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                       Jupyter RPC / Colab Protocol
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                   GOOGLE COLAB HOSTED GPU RUNTIME                      │
│                                                                        │
│  Hardware: NVIDIA Tesla T4 GPU (14.56 GB Dedicated VRAM)               │
│  Software: PyTorch 2.11.0+cu130, CUDA 13.0, cuDNN                      │
│  Execution: Tensor graph construction, forward/backward passes,        │
│             loss convergence, evaluation loops                         │
└────────────────────────────────────────────────────────────────────────┘
```

### Key Architectural Tenets
1. **Local Filesystem Sovereignty:** All source code, notebook files, protocols, metrics, and documentation live permanently on your local disk inside `/home/official/projects/icir-research`. They are version-controlled via local Git.
2. **Remote Compute Execution:** The heavy computational operations (matrix multiplications, GPU tensor allocations, autograd calculations) execute remotely on Google Colab's hosted Tesla T4 GPU.
3. **Seamless Result Synchronization:** Outputs generated during execution (tables, stdout, Matplotlib figures) stream directly back through the Jupyter protocol into VS Code, serializing immediately into your local `.ipynb` file upon save (`Ctrl+S`).

---

## 2. Dependency Separation Protocol

### Local Workstation Dependencies (`requirements.txt`)
The root `requirements.txt` is **strictly for your local Python environment**. It equips your local VS Code editor with:
- Jupyter kernel protocol adapters (`ipykernel`, `jupyterlab`, `notebook`)
- Static analysis & language server support (Pylance, Pyright)
- Offline CPU fallback capabilities

To configure locally:
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Google Colab Runtime Integrity
> [!CAUTION]
> **DO NOT run `!pip install -r requirements.txt` inside Google Colab.**  
> Google Colab's hosted GPU environment comes pre-installed with PyTorch 2.11.0+cu130, Torchvision, CUDA toolchains, NumPy, SciPy, and Matplotlib optimized specifically for the Tesla T4.  
> Unnecessarily running package managers inside Colab can break pre-configured CUDA bindings, corrupt binary dependencies, or trigger forced runtime restarts.

---

## 3. Step-by-Step Experiment Execution Lifecycle

### Step 1: Open Notebook Locally
Open the desired experiment notebook in VS Code:
```bash
code experiments/notebooks/EXP-2026-001.ipynb
```

### Step 2: Verify Kernel Connection
Look at the top-right corner of the VS Code notebook editor:
1. Click the **Kernel Selector** (top right).
2. Confirm it displays the connected Colab hosted runtime (e.g., `Python 3 (ipykernel)` on Colab).
3. If disconnected, select **Connect to Google Colab** or **Existing Jupyter Server** and re-authenticate without resetting the instance.

### Step 3: Run Runtime Verification
Execute Cell 2 and Cell 3 of the notebook. Confirm the terminal prints:
```
=================================================================
ICIR RUNTIME HARDWARE & ACCELERATOR AUDIT
=================================================================
CUDA Available     : True
Device Name        : Tesla T4
Dedicated VRAM     : 14.56 GB
Compute Capability : SM 7.5
CUDA Toolchain     : 13.0
cuDNN Version      : ...
STATUS: Verified connected to Google Colab GPU Runtime (Tesla T4).
=================================================================
```

### Step 4: Review Configuration & Seed Protocol
Verify that the hyperparameters in `ExperimentConfig` match the preregistered protocol in `experiments/protocols/EXP-2026-001-protocol.md`. Verify the canonical seed list:
```python
CANONICAL_SEEDS = [42, 1337, 2026, 7, 99]
```

### Step 5: Execute Training Regimes
Run the execution cells. Monitor training progress and validation metrics across:
- **Regime A (Naive Sequential Fine-Tuning):** Evaluates catastrophic forgetting on prior tasks.
- **Regime B (Joint Offline Oracle):** Evaluates multi-task upper bound.

### Step 6: Review Visualizations & Inspect Serialized Artifacts
- The notebook displays the $T \times T$ retention matrix $R$ and Task 1 degradation curves inline.
- Plots are saved to: `experiments/outputs/EXP-2026-001/figures/EXP-2026-001_retention_curves.png`.
- Structured metrics are exported to: `experiments/outputs/EXP-2026-001/metrics/EXP-2026-001_metrics.json`.

---

## 4. Git Version Control, Synchronization & Retention Policy

### 4.1 Git Tracking Rules

| Directory / Artifact Type | Tracked in Git? | Rationale |
| :--- | :---: | :--- |
| `experiments/notebooks/*.ipynb` | **Yes** | Code and analysis pipeline must be version-controlled. |
| `experiments/protocols/*.md` | **Yes** | Preregistered scientific hypothesis and empirical report. |
| `experiments/outputs/*/metrics/*.json` | **Yes** | Canonical empirical measurements and summary statistics. |
| `experiments/outputs/*/figures/*.png` | **Yes** | Publication-ready visual evidence. |
| `experiments/outputs/*/logs/*.log` | **No** (Ignored) | Console noise and transient progress bars. |
| Model Weights (`*.pt`, `*.pth`, `*.bin`) | **No** (Ignored) | Large binaries bloat Git history; store in S3/GCS. |
| Raw Datasets (`data/raw/*`) | **No** (Ignored) | Raw data must be fetched reproducibly via code. |

### 4.2 Git Commit & Synchronization Workflow

Once an experimental run concludes:

#### 1. Save Local Notebook
Press `Ctrl+S` in VS Code. This writes the executed cells, inline tables, and base64 plots directly to `experiments/notebooks/EXP-2026-001.ipynb`.

#### 2. Verify Output Files
Check that the outputs exist in the repository tree:
```bash
ls -la experiments/outputs/EXP-2026-001/metrics/
ls -la experiments/outputs/EXP-2026-001/figures/
```

#### 3. Update Master Experiment Register & Protocol
1. Open [`experiments/register.md`](register.md):
   - Update `Status` from `PLANNED` to `COMPLETED` (or `DISCONFIRMED` if hypothesis was rejected).
   - Enter the primary statistical finding (e.g., $A_T = 0.21 \pm 0.02, BWT = -0.74 \pm 0.03$).
2. Open [`experiments/protocols/EXP-2026-001-protocol.md`](protocols/EXP-2026-001-protocol.md):
   - Populate Section 5.2 (Actual Experimental Findings) with the measured values.
   - Record qualitative observations in Section 6.

#### 4. Stage and Commit Changes
Follow ICIR conventional commit semantics:
```bash
# Check modified files
git status

# Stage experiment protocol, outputs, notebook, and register
git add experiments/protocols/EXP-2026-001-protocol.md
git add experiments/notebooks/EXP-2026-001.ipynb
git add experiments/outputs/EXP-2026-001/
git add experiments/register.md

# Commit with precise semantic message
git commit -m "exp(EXP-2026-001): record empirical catastrophic forgetting baseline metrics"
```

#### 5. Record the Commit Hash
Record the resulting 7-character or 40-character commit hash (`git rev-parse HEAD`) into the `Git Commit` column of `experiments/register.md` and Section 1 of the protocol. This satisfies ICIR Rigor Pillar V (Git Pinning).

#### 6. Push to Remote Repository
Synchronize your local commits with your remote repository:
```bash
git push origin main
```

---

## 5. Troubleshooting & Defensive Practices

### What if the Colab session disconnects during execution?
1. Do not panic; your local `.ipynb` code is safe on your workstation.
2. In VS Code, reconnect to the Colab hosted runtime.
3. Re-execute the environment audit cell (Cell 3) to verify GPU allocation.
4. Because the Permuted MNIST dataset is cached in `./data`, re-running from Cell 8 takes negligible time.

### How to clean notebook outputs if git diff becomes too large?
If git diffs on `.ipynb` files become noisy:
- Preserve the evaluated notebook as the official run artifact.
- Alternatively, strip large output streams while preserving plots:
  ```bash
  # Check git diff summary without raw JSON metadata
  git diff --stat experiments/notebooks/EXP-2026-001.ipynb
  ```

### Determinism Verification
If two runs on the same seed produce diverging terminal metrics:
- Ensure `torch.backends.cudnn.deterministic = True` and `torch.backends.cudnn.benchmark = False` were set before any tensor allocation.
- Confirm `PYTHONHASHSEED` was set prior to any set/dict hashing.
- Verify no non-deterministic CUDA operations were invoked.

