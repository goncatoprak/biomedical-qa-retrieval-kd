# Biomedical QA Retrieval — Knowledge Distillation Reproducibility

Companion repository for *Preserving ranking geometry in biomedical question answering retrieval: A systematic study of knowledge distillation objectives* (PeerJ Computer Science manuscript).

## Quick start (Google Colab)

1. Download this repository using **Code → Download ZIP** and extract it. The notebook is designed for Google Colab with Google Drive mounted and access to a GPU; it is **not** a one-click local script.
2. Open `biomedical_kd_reproducibility.ipynb` in Colab. Review the first setup cell and replace `/content/drive/PROJECT_ROOT` with the Google Drive folder you intend to use. The notebook contains additional references to `PROJECT_ROOT`; search the notebook for that literal string and adjust paths consistently.
3. Place the included `splits/*.csv` files in the notebook's `fixed_positive_splits/` directory, or change `SPLIT_DIR` to point to the repository's `splits/` directory. Place the original source data (including `MedQuad.csv` as expected by the notebook) under the configured `data/` directory when running preprocessing. Original dataset download links are provided in the manuscript; this package does not include every raw source file.
4. Run notebook setup and preprocessing cells in order. Review the code before executing expensive training cells. The full experiment switches `RUN_MAIN_EXPERIMENTS`, `RUN_BASELINES`, `RUN_ALPHA_SWEEP`, and `RUN_MULTISEED` are set to `False` by default; set only the relevant switches to `True` for the experiments being reproduced.
5. Compare outputs with `results/reported_retrieval_results.csv`. The controlled five-seed configuration is described in `configs/experiments.json`.

**Important reproducibility limitation:** The supplied notebook contains Colab-specific paths and exploratory/audit sections. It has not been executed end-to-end in a fresh environment as part of preparing this repository. Some analyses depend on previously generated intermediate files. Consequently, the archived results are provided for traceability, not as proof of a verified one-click reproduction.

## Environment

The notebook uses Python, PyTorch, Hugging Face Transformers, pandas, NumPy, SciPy, scikit-learn and other packages imported in its cells. Use a Colab GPU runtime; package versions and hardware can affect reproducibility. Inspect the notebook imports and install any missing dependencies in the runtime. No independently verified pinned environment is supplied in this archive.

## Repository layout

- `biomedical_kd_reproducibility.ipynb`: implementation and analyses
- `splits/`: fixed positive QA pairs and SHA-256 manifest
- `configs/experiments.json`: experiment objectives, seeds and hyperparameters
- `results/reported_retrieval_results.csv`: archived controlled results
- `file_inventory.csv`: archive inventory

## Data provenance and responsible reuse

The study uses PubMedQA, BioASQ and MedQuAD. Review each source dataset's license and redistribution terms before reusing the included processed splits. Do not upload API tokens, personal Drive paths, credentials or private patient information.

---

## Original supplement notes

# Biomedical QA Dense Retrieval — Reproducibility Supplement

This package provides the minimal materials needed to trace and reproduce the controlled experiments reported in the revised manuscript.

## Contents

- `biomedical_kd_reproducibility.ipynb` — single end-to-end implementation of preprocessing, negative sampling, training, model selection, and evaluation. Notebook outputs were cleared for submission.
- `splits/` — fixed positive-pair train, validation, and test splits used after overlap control. `split_manifest.json` records counts, hashes, the split seed, and the near-duplicate threshold using relative paths only.
- `configs/experiments.json` — machine-readable definition of the final training setup, KD objectives, temperatures, alpha convention, and five random seeds.
- `results/reported_retrieval_results.csv` — machine-readable per-seed retrieval results for the controlled PubMedBERT → DistilBERT comparison of NoKD, K1, K2, and K3 (alpha=0.10). It contains dataset-specific and macro test metrics.

## Controlled multi-seed experiment

Random seeds: 42, 52, 62, 72, 82.

Teacher: `microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract`

Student: `distilbert-base-uncased`

The reported K3 multi-seed setting uses alpha=0.10. Alpha weights the embedding-alignment term, while `(1-alpha)` weights the teacher-ranking distillation term.

All strategies share the same supervised candidate-ranking objective. K1 adds embedding alignment, K2 adds teacher-derived ranking-distribution supervision, and K3 combines both distillation signals.

## Fixed split integrity

The manifest contains SHA-256 hashes for all three fixed split files. Paths are relative so the supplement contains no author-specific Google Drive location.

## Result traceability

`reported_retrieval_results.csv` is intentionally limited to the controlled final multi-seed retrieval results rather than exploratory runs, obsolete intermediate experiments, model checkpoints, or audit artifacts. Each row identifies the strategy, seed, source dataset, candidate/query counts, and reported retrieval metrics.

The notebook and configuration file define the code path and hyperparameters used for the final controlled experiment, while the fixed split files make the data partition explicit.
