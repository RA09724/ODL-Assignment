# ODL-Assignment

Optimization and deep learning assignment — used-car price prediction on the Craigslist
cars & trucks dataset.

## Dataset setup

The CSVs are **not** tracked in this repo (1.4 GB and 1.0 GB — well over GitHub's 100 MB
per-file limit). To reproduce:

1. Download `vehicles.csv` from
   [Craigslist Used Cars (Kaggle)](https://www.kaggle.com/datasets/austinreese/craigslist-carstrucks-data).
2. Place it at `dataset/vehicles.csv`.
3. Run [`data_cleaning.ipynb`](data_cleaning.ipynb) — it writes `dataset/vehicles_cleaned.csv`,
   which the modelling notebooks consume.

## Notebooks

Run in this order:

| Notebook | Purpose | Reads |
| --- | --- | --- |
| [`data_cleaning.ipynb`](data_cleaning.ipynb) | Cleaning; produces the cleaned CSV | `dataset/vehicles.csv` |
| [`eda.ipynb`](eda.ipynb) | Exploratory analysis (samples rows via `SAMPLE_SIZE`) | `dataset/vehicles.csv` |
| [`model_building_tuning_evaluation.ipynb`](model_building_tuning_evaluation.ipynb) | Model building, Keras Tuner search, evaluation | `dataset/vehicles_cleaned.csv` |

`kt_dir/` (Keras Tuner search results) and `eda_outputs/` are generated when the notebooks
run and are likewise untracked.
