# Data Preprocessing for Machine Learning

This repository collects scripts and notebooks demonstrating **data preprocessing** techniques used before training machine learning models.  
It focuses on cleaning, transforming, and encoding raw data into a format suitable for modeling.

---

## Objectives

- Load tabular data and inspect its structure.
- Handle missing values (imputation, dropping, or encoding).
- Detect and treat outliers.
- Encode categorical variables (one‑hot, label encoding, etc.).
- Scale and normalize numeric features.
- Perform basic feature engineering and selection.

---

## Requirements

Typical stack (adjust to your environment):

```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```

You can also use `jupyter` or `jupyterlab` for running notebooks:

```bash
pip install jupyterlab
```

---

## How to Run

1. Clone or download the repository.
2. Start Jupyter Lab or Notebook:

   ```bash
   jupyter lab
   # or
   jupyter notebook
   ```

3. Open the notebooks in order (`01_exploration` → `02_cleaning` → `03_encoding_scaling` → etc.).
4. Run the cells and inspect the intermediate datasets to see how each preprocessing step changes the data.

---

## Integration with Machine Learning

Once preprocessing steps are defined:

- Export cleaned data to a new CSV for use in other projects.
- Wrap the transformations into a `sklearn` pipeline so that:
  - The same steps are applied consistently to training and test data.
  - The pipeline can be saved with `joblib` and reused in production.

This repository can serve as a template to structure preprocessing in future machine learning projects or teaching material on data preparation.
