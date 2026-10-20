# Stroke Classification Experiments: Source Code and Analysis

This repository contains Jupyter notebooks for the experiments reported in the study on stroke classification under class imbalance. The notebooks are organized into four experimental groups:

1. **Ablation**: Baseline, SMOTE, Genetic Algorithm (GA), and SMOTE+GA.
2. **Comparative SMOTE**: comparison of SMOTE-based resampling variants.
3. **Control Ratio**: experiments using different target class ratios.
4. **Optimizer Comparison**: comparison of Genetic Algorithm (GA) and Random Search (RS).

The repository contains both experiment notebooks and notebooks for descriptive/statistical analysis. These serve different purposes: experiment notebooks train/evaluate models and generate fold-level results, while analysis notebooks read existing result CSV files and summarize or statistically compare them.

## Repository Structure

```text
Code/
├── Ablation/
│   ├── ablation-baseline.ipynb
│   ├── ablation-ga.ipynb
│   ├── ablation-over.ipynb
│   ├── ablation-over-ga.ipynb
│   └── Ablation_Descriptive_Analysis_Per_Metric.ipynb
├── Comparative Smote/
│   ├── comparative-smote.ipynb
│   └── Comparative_SMOTE_Descriptive_Statistical_Analysis.ipynb
├── Control Ratio/
│   ├── control-ratio.ipynb
│   └── control_ratio_descriptive_statistical_analysis.ipynb
└── Optimizer Comparison/
    ├── ga-vs-rs.ipynb
    └── GA_vs_RS_Descriptive_Statistical_Analysis.ipynb
```

The filenames above are shown in a readable form. Use the actual filenames in your local copy if they differ in capitalization or punctuation.

## Requirements

The notebooks use Python and common scientific-computing and machine-learning packages, including:

- `numpy`
- `pandas`
- `scikit-learn`
- `scipy`
- `matplotlib`
- `xgboost`
- `imbalanced-learn`
- `smote-variants`
- `sklearn-genetic-opt`
- `statsmodels` (used by some analysis notebooks)

Not every notebook requires every package. Some experiment notebooks contain package-installation cells. You can run those cells in an environment where package installation is permitted, or install the required packages before running the notebook.

For a local environment, a typical setup is:

```bash
python -m venv .venv
```

Activate the environment, then install the packages required by the notebook you intend to run. The commands for activating a virtual environment differ between Windows, macOS, and Linux. The notebooks do not currently provide a single pinned `requirements.txt`, so package versions should be recorded separately if exact environment reproduction is required.

## Dataset

The experiment notebooks use the **Stroke Prediction Dataset** file named:

```text
healthcare-dataset-stroke-data.csv
```

Some notebooks contain a Kaggle-specific dataset path. If you run them outside Kaggle, update the `DATA_PATH` value or the `pd.read_csv(...)` path near the data-loading section to point to your local copy of the CSV file.

Do not commit the dataset to this repository unless its license and redistribution terms have been checked. The notebooks expect the raw CSV to be available at the configured path.

## How to Run the Experiments

Run the notebooks in the relevant folder using Jupyter Notebook, JupyterLab, or another environment that supports `.ipynb` files.

### 1. Ablation

Run the experiment notebooks as needed:

- `ablation-baseline.ipynb`: baseline without resampling or GA optimization.
- `ablation-over.ipynb`: resampling experiment using SMOTE.
- `ablation-ga.ipynb`: GA-based hyperparameter optimization without resampling.
- `ablation-over-ga.ipynb`: combined resampling and GA experiment.

The descriptive/statistical analysis notebook, `Ablation_Descriptive_Analysis_Per_Metric.ipynb`, does **not** retrain the models. It expects these result files in its working directory:

- `baseline_repeated_fold_results.csv`
- `smote_fold_results.csv`
- `ga_fold_results.csv`
- `smote_ga_fold_results.csv`

Run the corresponding experiment notebooks and ensure that the generated CSV files have these names and are located where the analysis notebook expects them. If the files are stored elsewhere, update the input configuration in the analysis notebook.

The analysis notebook saves tables, plots, and related outputs in `ablation_descriptive_outputs/`.

### 2. Comparative SMOTE

Run `comparative-smote.ipynb` to execute the comparative resampling experiment. Check its configuration and output section for the names and locations of generated files.

Then run `Comparative_SMOTE_Descriptive_Statistical_Analysis.ipynb` to analyze existing results. This analysis notebook expects the following files in its working directory:

- `comparative_smote_fold_results.csv`
- `comparative_smote_resampling_counts.csv`

It saves tables and figures in `comparative_smote_analysis_outputs_v2/`.

### 3. Control Ratio

Run `control-ratio.ipynb` to execute the controlled class-ratio experiment. Check the notebook's data path, experiment configuration, checkpoint settings, and output paths before running it.

Then run `control_ratio_descriptive_statistical_analysis.ipynb`. This analysis notebook expects:

- `control_ratio_fold_results.csv`

Place the CSV in the notebook's working directory or update `INPUT_FILE` in the notebook. The analysis outputs are saved in `control_ratio_analysis_outputs/`.

### 4. Optimizer Comparison: GA vs. Random Search

Run `ga-vs-rs.ipynb` to perform the optimizer comparison. Check the configured dataset path, output directory, and checkpoint behavior before execution.

Then run `GA_vs_RS_Descriptive_Statistical_Analysis.ipynb`. It expects:

- `ga_rs_fold_results.csv`

Place the CSV in the analysis notebook's working directory or update `INPUT_FILE`. The analysis outputs are saved in `ga_rs_analysis_output/`.

## Running the Analysis Notebooks

Analysis notebooks operate on CSV results generated by the corresponding experiment notebooks. They do not automatically locate or regenerate missing source results. If a required CSV is missing, check the experiment output directory and update the input path or copy the required file into the analysis notebook's working directory.

For reliable analysis:

1. Confirm that each input CSV comes from the intended experiment run.
2. Check the column names and the number of observations loaded by the notebook.
3. Review any data-validation warnings or errors before interpreting the output.
4. Keep the raw fold-level result files so that the reported summaries can be traced back to the experiment outputs.

## Reproducibility Notes

- Review each notebook's configuration cells before execution, particularly dataset paths, random seeds, cross-validation settings, hyperparameter search settings, and output paths.
- Some notebooks save checkpoints or append results to existing CSV files. Inspect the checkpoint/output settings before rerunning an experiment to avoid unintentionally mixing results from different runs.
- Do not combine CSV files from different configurations unless that combination is intended and documented.
- The analysis notebooks calculate summaries and statistical tests from the supplied result files. Their outputs depend on the contents and structure of those files.
- The analysis notebooks may produce descriptive confidence intervals from cross-validation scores. Such intervals should be interpreted in light of the dependence that can arise between repeated cross-validation results.
- The notebooks are provided as research code. Their results should not be interpreted as evidence of clinical readiness.

## Troubleshooting

**`FileNotFoundError` for the dataset**  
Update the notebook's dataset path to the location of `healthcare-dataset-stroke-data.csv`.

**A required result CSV is missing**  
Run the corresponding experiment notebook first, confirm where it saves its output, and make sure the analysis notebook points to that file.

**A notebook cannot import a package**  
Install the package required by that notebook in the active Python environment, then restart the kernel if necessary.

**Results appear to be duplicated or inconsistent after rerunning**  
Check whether the notebook resumes from checkpoints or appends to an existing CSV. Use a separate output location or carefully manage old result files when starting a new experiment.

## Scope

This repository documents the code workflow for the four experiment groups and their associated analyses. Exact reproduction depends on using the same dataset, configuration, package versions, random seeds, and input result files as the original runs.
