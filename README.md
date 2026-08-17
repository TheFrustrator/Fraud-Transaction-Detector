# Fraud Transaction Detector

A machine learning-based detector for identifying potentially fraudulent financial transactions. This repository contains data preprocessing, model training, evaluation scripts, and a simple inference API to classify transactions as legitimate or fraudulent.

## Features

- Data preprocessing and feature engineering pipelines
- Several model training options (e.g., XGBoost, Random Forest, logistic regression)
- Model evaluation and reporting (ROC AUC, precision, recall, F1)
- Command-line and API-based inference
- Dockerfile for reproducible deployments

## Project structure

- data/              - raw and processed data (not included in repo)
- notebooks/         - exploratory analysis and experiments
- src/               - source code (preprocessing, models, training, inference)
- models/            - trained model artifacts
- tests/             - unit and integration tests
- Dockerfile         - container definition for running the app
- requirements.txt   - Python dependencies
- README.md          - this file

## Quickstart

Prerequisites:
- Python 3.8+
- pip
- (Optional) Docker

1. Clone the repo

```bash
git clone https://github.com/TheFrustrator/Fraud-Transaction-Detector.git
cd Fraud-Transaction-Detector
```

2. Create a venv and install dependencies

```bash
python -m venv .venv
source .venv/bin/activate   # on Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

3. Prepare data

Place your dataset CSV(s) into the data/raw/ directory. Expected columns should include transaction amount, timestamp, user identifiers, and a label column named `is_fraud` (0 = legitimate, 1 = fraud). See `src/data/README.md` or the preprocessing script for exact requirements.

4. Train a model (example)

```bash
python src/train.py --config configs/xgb_config.yaml --output models/xgb_model.pkl
```

5. Evaluate

```bash
python src/evaluate.py --model models/xgb_model.pkl --data data/processed/test.csv
```

6. Run inference (local API)

```bash
# Start simple FastAPI server (example)
uvicorn src.app:app --reload --host 0.0.0.0 --port 8000

# Call inference
curl -X POST "http://127.0.0.1:8000/predict" -H "Content-Type: application/json" -d '{"transaction": {"amount": 123.45, "feature_1": 0.5, "feature_2": 1}}'
```

## Docker

Build and run the container:

```bash
docker build -t fraud-detector:latest .
docker run -p 8000:8000 fraud-detector:latest
```

## Configuration

Configurations such as training hyperparameters, feature lists, and paths are stored in the `configs/` directory as YAML files. Adjust them to match your dataset and resources.

## Testing

Run tests with pytest:

```bash
pytest -q
```

## Contributing

Contributions are welcome. Please open an issue to discuss major changes, and submit pull requests with clear descriptions and tests.

## Security & Data Privacy

- Do not commit sensitive data (PII, full transaction logs) into the repository.
- Follow your organization's data handling policies.

## License

This project is licensed under the MIT License. See LICENSE for details.

## Contact

Maintained by TheFrustrator. Open issues or PRs for questions and suggestions.
