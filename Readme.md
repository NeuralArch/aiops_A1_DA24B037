# AIOps — Module 1 Assignment: Experiment Management & Reproducibility

**Name:** Ayush Kanojiya
**Roll No:** DA24B037

This repository contains the code, experiment logs, and data-versioning history for Module 1 of the AI Operations (AIOps) course, covering technical debt diagnosis, MLflow experiment tracking, DVC data versioning, and an end-to-end reproducibility drill.

See `AI_DISCLOSURE.md` for details on AI tool usage in this assignment.

---

## Repository Structure

```
.
├── q2.ipynb              # Question 2 — MLflow experiment tracking (MLP on MNIST)
├── train.py               # Question 4 — Partner A training + MLflow logging script
├── requirements.txt        # Python dependencies
├── file_list.csv           # DVC-tracked dataset file listing (versioned v1 → v2)
├── file_list.csv.dvc        # DVC pointer file (committed alongside code)
├── .dvc/                  # DVC configuration (SSH remote storage)
├── mlflow.db               # Local MLflow tracking store
├── mlartifacts/             # Logged MLflow model artifacts
└── data/                  # Training/validation image data (DVC-tracked)
```

---

## Question 1 — Technical Debt Diagnosis
Conceptual analysis of hidden technical debt in a campus food-delivery recommendation system, covering entanglement between unrelated features, undeclared consumers of model output, and undocumented pipeline glue code. See the written 1-page PDF submission for the full response.

## Question 2 — MLflow Experiment Comparison
`q2.ipynb` trains an `MLPClassifier` on the MNIST dataset across **six runs**, varying:
- `hidden_layer_sizes`: (32,), (64,), (128, 64)
- `learning_rate_init`: 0.001, 0.01

Each run logs the following to MLflow:
- **Params:** `hidden_layer_sizes`, `learning_rate_init`, `dataset`, `model_type`
- **Metrics:** `train_accuracy`, `val_accuracy`, `train_loss`, `train_loss_curve` (per-epoch)
- **Artifact:** the trained model, logged with an inferred input/output signature via `mlflow.sklearn.log_model(..., signature=sig)`

Run comparison, best-performing configuration, and overfitting analysis are documented in the written analysis submitted alongside this repo.

## Question 3 — DVC Data Versioning & Rollback
Demonstrates dataset versioning using DVC with an SSH remote:

1. **v1** — `file_list.csv` generated from the initial dataset (1800 files → 1801 rows incl. header), tracked with `dvc add`, committed with code, tagged `v1`, and pushed to remote.
2. **v2** — Simulated a data update (new images added, bringing the file count to 2801), regenerated `file_list.csv` (2801 rows), re-added via `dvc add`, committed as `v2`, tagged, and pushed.
3. **Rollback** — Demonstrated reverting to `v1` using `git checkout v1` + `dvc checkout`, with terminal output (`wc -l file_list.csv`) confirming the restored row count exactly matches the original v1 snapshot.

## Question 4 — Capstone: End-to-End Reproducibility Drill (Partner A)


Repo Link - https://github.com/NeuralArch/Q4_DA24B037_DA24B038.git

`train.py` implements the **Partner A** portion of the reproducibility protocol:

- Trains a `RandomForestClassifier` on the Iris dataset with a fixed seed (`SEED = 42`)
- Logs to MLflow:
  - **Params:** `n_estimators`, `max_depth`, `random_state`, `seed`
  - **Metric:** `accuracy`
  - **Tags:** `git_commit` (current commit hash via `git rev-parse HEAD`), `dvc_data_version` (hash of the `.dvc` pointer file)
  - **Artifact:** the trained model via `mlflow.sklearn.log_model`
- Commits `train.py`, `requirements.txt`, and `file_list.csv.dvc` together in a single Git commit, tagged `model-v1`
- Registers the model as `IrisClassifier` and transitions it to the **Staging** stage in the MLflow Model Registry (`promote_to_staging.py`)

---

## Setup & Reproduction

```bash
# Clone and enter the repo
git clone <repo-url>
cd DA24B037_AIOps_Assignment1

# Create environment
python3 -m venv myenv
source myenv/bin/activate
pip install -r requirements.txt

# Restore DVC-tracked data
dvc pull

# Start the MLflow tracking server (in a separate terminal)
mlflow server --host 0.0.0.0 --port 5000

# Run training
python3 train.py
```

---

## Video Walkthrough
A short screen-recorded walkthrough (4–5 minutes) covering Questions 2, 3, and 4 is submitted alongside this repository, demonstrating live execution of the MLflow experiment loop, DVC versioning/rollback, and the Partner A training + registration workflow.