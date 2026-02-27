# EEG Brain Signal → Text (Notebook Pipeline)

This repo contains a complete **end-to-end notebook pipeline** that loads EEG features from ZuCo-style MATLAB `.mat` files, builds a model-ready dataset, splits train/test by subject, visualizes the data, and trains a baseline EEG→Text model in PyTorch.

> ✅ Main artifacts in this repo are the `.ipynb` notebooks listed below.

---

## What’s inside

### 1) `data_loader.ipynb` — Build the dataset from MATLAB files
**Goal:** Parse subject `.mat` files and create aligned EEG + text pairs.

What it does:
- Reads MATLAB `.mat` files (HDF5 format) from a folder like `Matlabfiles/`
- Extracts sentence text + per-word EEG features
- Uses EEG feature keys:
  - `FFD_a1, FFD_a2, FFD_b1, FFD_b2, FFD_g1, FFD_t1`
- Normalizes per feature (mean/std) and creates one EEG vector per word
- Saves outputs:
  - `zuco2_nr_eeg_text_pairs.pkl` (raw pairs)
  - `zuco2_nr_eeg_ready.pkl` (model-ready)
  - `zuco2_nr_eeg_ready.csv` (human-readable export)

📌 Expected input:
- Put your `.mat` files inside:

---

### 2) `data_spliter.ipynb` — Train/Test split by subject + test samples export
**Goal:** Create reproducible splits and export test samples.

What it does:
- Loads the ready dataset
- Splits by subject ID (example test subjects):
- `["YAC", "YDR"]`
- Saves:
- `training.pkl`
- `testing.pkl`
- Also exports each test row into:

---

### 3) `DataVisualization.ipynb` — Data exploration & plots
**Goal:** Quick EDA to understand distributions and structure.

Uses:
- `pandas`, `numpy`
- `matplotlib`, `seaborn`

---

### 4) `egg-to-text.ipynb` — Model training (EEG → Text)
**Goal:** Train a baseline deep learning model for predicting text tokens from EEG sequences.

Includes:
- PyTorch Dataset/DataLoader
- Vocabulary building (Counter)
- Model: **Linear EEG encoder + positional embeddings + TransformerEncoder**
- Training + evaluation loop
- Inference helper (sentence prediction)
