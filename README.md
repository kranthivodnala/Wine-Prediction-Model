# Wine Prediction Model

A machine learning project that predicts wine quality using a Random Forest regression model with MLflow integration for experiment tracking.

## Project Overview

This project demonstrates a complete MLOps workflow for building and tracking a wine quality prediction model. It uses the wine dataset to train a Random Forest model and logs all parameters, metrics, and artifacts using MLflow.

## Features

- **Random Forest Regression**: Predicts wine quality based on physicochemical properties
- **MLflow Integration**: Tracks experiments, parameters, and metrics
- **Configurable Parameters**: Adjust model hyperparameters via command-line arguments
- **Data Versioning**: DVC integration for dataset management
- **Modular Code**: Utility functions for data loading and preprocessing

## Project Structure

```
├── README.md                 # Project documentation
├── requirements.txt          # Python dependencies
├── train.py                  # Main training script with MLflow logging
├── utils.py                  # Utility functions for data handling
└── data/
    ├── wine_sample.csv       # Wine dataset
    └── wine_sample.csv.dvc   # DVC data tracking file
```

## Requirements

- Python 3.7+
- pandas
- scikit-learn
- joblib
- numpy
- mlflow

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd Wine-Prediction-Model
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. (Optional) Set up MLflow tracking server:
```bash
mlflow ui
```

## Data Versioning with DVC

This project uses DVC (Data Version Control) for managing and versioning datasets. Follow these steps to set up DVC:

### 1. Create and Activate Virtual Environment
```bash
python -m venv .venv
source .venv/Scripts/activate
```

### 2. Install DVC
```bash
python -m pip install dvc
```

### 3. Initialize DVC
```bash
python -m dvc init
```

### 4. Add Data to DVC
Track your dataset with DVC:
```bash
python -m dvc add data/wine_sample.csv
```

This creates a `wine_sample.csv.dvc` file:
```bash
cat wine_sample.csv.dvc
```

### 5. Configure Remote Storage (S3)
Add a remote S3 bucket for data storage:
```bash
python -m dvc remote add -d wineremote s3://wine-report-personallabs
```

### 6. Install DVC S3 Extension
```bash
python -m pip install dvc_s3
```

### 7. Push Data to Remote
Upload your tracked data to the remote storage:
```bash
python -m dvc push
```

## Usage

### Basic Training

Run the training script with default parameters:

```bash
python train.py
```

### Advanced Training

Customize model parameters:

```bash
python train.py \
  --csv data/wine_sample.csv \
  --target quality \
  --experiment wine-prediction \
  --run run-2 \
  --n-estimators 100 \
  --max-depth 10 \
  --test-size 0.2 \
  --random-state 42
```

### Command-Line Arguments

| Argument | Default | Description |
|----------|---------|-------------|
| `--csv` | `data/wine_sample.csv` | Path to CSV dataset |
| `--target` | `quality` | Target column name |
| `--experiment` | `wine-prediction` | MLflow experiment name |
| `--run` | `run-2` | MLflow run name |
| `--n-estimators` | `50` | RandomForest number of trees |
| `--max-depth` | `5` | RandomForest max tree depth |
| `--test-size` | `0.2` | Fraction of data for testing (0-1) |
| `--random-state` | `42` | Random seed for reproducibility |

## Model Details

### Algorithm
- **Model**: Random Forest Regressor
- **Task**: Regression (predicting continuous quality scores)

### Metrics

The model logs the following metrics:
- **MSE** (Mean Squared Error)
- **RMSE** (Root Mean Squared Error)
- **R²** (Coefficient of Determination)

### Data Split
- Training set: 80% (default)
- Test set: 20% (default)

## MLflow Integration

The training script automatically:
1. Sets up MLflow tracking (local or remote)
2. Creates/uses the specified experiment
3. Logs all hyperparameters
4. Logs training/test data sizes
5. Logs model performance metrics
6. Tracks model artifacts

### Accessing Results

View the MLflow UI:
```bash
mlflow ui
```

Then navigate to `http://localhost:5000` to explore experiments and runs.

## Environment Variables

- `MLFLOW_TRACKING_URI`: MLflow tracking server URI (default: `http://localhost:7006`)

## Data

The `wine_sample.csv` dataset contains:
- Wine physicochemical properties (features)
- Wine quality (target variable, 0-10 scale)

Data is versioned using DVC (see `wine_sample.csv.dvc`).

## License

This project is for educational and practice purposes.
