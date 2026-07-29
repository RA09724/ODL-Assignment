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

## Cleaned dataset schema

`data_cleaning.ipynb` writes 18 columns. The ones that differ from the raw Kaggle file:

| Column | Notes |
| --- | --- |
| `price` | Filtered to `[500, 57364]` — the $500 floor removes dealer bait listings ($1, $123) that otherwise dominate MAPE |
| `price_log` | `log1p(price)`; the training target. Invert with `expm1` before computing any metric, so results stay in dollars |
| `car_age` | `scrape_year − year`, where the scrape year is read off `posting_date` before that column is dropped |
| `model` | Text-normalised, then collapsed to the top `TOP_N_MODELS` (500) names plus `other` — a ~501-row embedding table instead of 29,667 |
| `cylinders_num` | Integer extracted from `"6 cylinders"`, so the ordinal relation 4 < 6 < 8 survives |
| `cylinders_unknown` | Flag for rows whose `cylinders` was `"other"` or missing |

Removed: `VIN` (used for deduplication first — dealers repost the same car across regions, which
leaks between the train and test splits), `description` and `posting_date` (free text; the
description also leaks the price), `size` (72% missing), `lat`/`long` (median imputation invented
locations; `region`/`state` already encode geography), and the identifier/URL columns.

Scalers and encoders are deliberately **not** fitted in the cleaning notebook — the modelling
notebook splits first and fits them on the training split only.
