# Heart Disease Risk: Machine Learning, Clustering, and PCA

An end-to-end machine-learning project using a synthetic heart-disease risk dataset. The notebook covers data quality checks, exploratory analysis, supervised classification, hyperparameter tuning, error analysis, K-Means clustering, hierarchical clustering, Gaussian mixture models, and dimensionality reduction with PCA.

> **Medical disclaimer:** This project is educational. The dataset is synthetic, and the results must not be used for diagnosis, screening, treatment, or other medical decisions.

## Project Structure

```text
heart-disease-ml-analysis/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md
│   └── raw/
│       └── heart_disease_risk_2026.csv
└── notebooks/
    └── heart_disease_analysis.ipynb
```

### Where Each File Goes

| File | Location | Purpose |
| --- | --- | --- |
| `README.md` | Repository root | Explains the project, results, setup, and structure |
| `requirements.txt` | Repository root | Lists the Python dependencies needed to run the notebook |
| `.gitignore` | Repository root | Prevents virtual environments, caches, and notebook checkpoints from being committed |
| `data/README.md` | `data/` | Documents the dataset source, license, location, and disclaimer |
| `heart_disease_risk_2026.csv` | `data/raw/` | Source dataset used by the notebook |
| `heart_disease_analysis.ipynb` | `notebooks/` | The cleaned and organized analysis notebook |

Do not place the CSV beside the notebook. The notebook resolves the repository root and loads it from `data/raw/`. If the CSV is missing, it attempts to download the public Kaggle dataset and restore it to that location.

## Dataset

The project uses [Heart Disease Risk 2026 on Kaggle](https://www.kaggle.com/datasets/uditjain13/heart-disease-risk-2026).

- 9,000 observations
- 27 columns
- Binary target: `has_heart_disease`
- Synthetic health, lifestyle, laboratory, and wearable-device features
- Dataset license: CC0 — Public Domain

The target contains moderate class imbalance: approximately 30.3% positive cases and 69.7% negative cases.

## Workflow

1. Validate the schema, missing values, duplicates, identifiers, and target values.
2. Explore distributions, group differences, and numerical correlations.
3. Split the data into stratified training and test sets.
4. Build leakage-safe preprocessing and model pipelines.
5. Compare Logistic Regression, SVM, Random Forest, and Gradient Boosting with five-fold cross-validation.
6. Tune model hyperparameters using only the training data.
7. Evaluate the selected model once on the untouched test set.
8. Analyze false positives and false negatives.
9. Compare K-Means, hierarchical clustering, and GMM.
10. Reduce the encoded feature space with PCA while retaining at least 95% of its variance.

## Key Results

### Supervised Learning

Logistic Regression produced the strongest balance of validation performance, generalization, interpretability, and efficiency.

| Test metric | Value |
| --- | ---: |
| Accuracy | 0.8978 |
| Precision | 0.8632 |
| Recall | 0.7872 |
| F1 score | 0.8234 |
| ROC-AUC | 0.9497 |

The confusion matrix contained 1,187 true negatives, 429 true positives, 68 false positives, and 116 false negatives. False negatives are the main weakness and would require special attention in any real risk-screening research.

### Clustering

| Algorithm | Silhouette ↑ | Davies–Bouldin ↓ | Calinski–Harabasz ↑ |
| --- | ---: | ---: | ---: |
| K-Means | 0.0947 | 2.8941 | 1039.6846 |
| Hierarchical | 0.0574 | 3.3166 | 584.6316 |
| GMM | 0.0323 | 5.9850 | 173.4890 |

K-Means performed best among the tested methods, but its low silhouette score indicates considerable overlap. The clusters are exploratory segments, not strongly separated natural groups.

### PCA

- Original encoded feature count: 31
- Components retained at the 95% threshold: 21
- Variance preserved: 95.42%

The notebook uses the 21-component representation for the before/after clustering comparison. A separate two-component PCA projection is used only for visualization.

## Installation

Clone the repository and enter its folder:

```bash
git clone https://github.com/YOUR_USERNAME/heart-disease-ml-analysis.git
cd heart-disease-ml-analysis
```

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell, activate it with:

```powershell
.venv\Scripts\Activate.ps1
```

Install the dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Start JupyterLab:

```bash
jupyter lab notebooks/heart_disease_analysis.ipynb
```

Run the sections you need from top to bottom. Model tuning is intentionally separated because it is the slowest section. The notebook defaults to one processing worker for compatibility with constrained environments. On a suitable machine, more workers can be enabled before starting Jupyter:

```bash
export N_JOBS=-1
```

On Windows PowerShell:

```powershell
$env:N_JOBS = "-1"
```

## Uploading to GitHub

### GitHub Website

1. Create a new empty repository named `heart-disease-ml-analysis`.
2. Open the new repository and choose **Add file → Upload files**.
3. Upload the **contents** of the local `heart-disease-ml-analysis` folder so that `README.md` appears at the repository root.
4. Confirm that the `data/` and `notebooks/` folders match the structure shown above.
5. Commit the upload.

### Git Command Line

From inside the project folder:

```bash
git init
git add .
git commit -m "Add heart disease machine learning analysis"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/heart-disease-ml-analysis.git
git push -u origin main
```

## Limitations

- The data is synthetic and may not represent real clinical populations.
- The model has not been externally validated.
- Cluster separation is weak according to the internal validation metrics.
- Correlation and feature importance do not establish causality.
- The test results describe this dataset and experimental setup only.

## License Notes

The dataset is released under CC0. No separate license has been assigned to the project code; add a repository license if you want to define how others may reuse it.
