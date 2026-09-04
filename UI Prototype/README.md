## Quick Links

<p align="center">
  <a href="https://fundfirst-dream-planner.lovable.app"><b>Launch FundFirst</b></a> •
  <a href="https://github.com/sarahnish/FundFirst_backend"><b>View FastAPI Backend</b></a> •
  <a href="https://github.com/sarahnish/fund-first/blob/main/UI%20Prototype/homepage.png"><b>Homepage Screenshot</b></a> •
  <a href="https://github.com/sarahnish/fund-first/blob/main/UI%20Prototype/results.png"><b>Results Screenshot</b></a>
</p>

## Deployment Architecture

FundFirst uses three separate components:

1. **scikit-learn model pipeline** — the fitted `StandardScaler` and Logistic Regression model are saved together as a reusable `.joblib` pipeline.
2. **FastAPI backend** — loads the saved pipeline, validates incoming inputs and returns the predicted feasibility class and class probabilities.
3. **Lovable frontend** — provides the user interface and sends prediction requests to the deployed API over HTTPS.

```text
User enters scenario
        ↓
Lovable frontend
        ↓
HTTPS POST /predict
        ↓
FastAPI input validation
        ↓
Saved scikit-learn pipeline
StandardScaler + Logistic Regression
        ↓
Prediction + class probabilities
        ↓
JSON response
        ↓
Lovable displays result
```

The frontend handles the user experience, while preprocessing and model inference remain in the Python backend.

The Lovable frontend does not recreate the trained model, scaling logic or prediction rules. The FastAPI service remains the source of truth for live predictions.
