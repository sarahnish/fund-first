## Deployment Architecture

FundFirst is deployed as a small full-stack machine-learning application with three separate components:

1. **scikit-learn model pipeline** — the fitted `StandardScaler` and Logistic Regression model are saved together as a reusable `.joblib` pipeline.
2. **FastAPI backend** — loads the saved pipeline, validates incoming inputs and returns the predicted feasibility class and class probabilities.
3. **Lovable frontend** — provides the user interface and sends prediction requests to the deployed API over HTTPS.

<p align="center">
  <b>Lovable UI → FastAPI → scikit-learn pipeline → prediction + probabilities → Lovable UI</b>
</p>

The frontend does not reproduce the model or preprocessing logic. Model inference remains on the Python backend, keeping the saved model pipeline and preprocessing steps server-side.

[Launch FundFirst →](https://fundfirst-dream-planner.lovable.app)

### Prediction Flow

When a user enters a scenario, the frontend sends an HTTPS `POST` request to the FastAPI `/predict` endpoint.

Example request:

```json
{
  "AveragePrice": 515000,
  "MedianAnnualPay": 35500,
  "SavingRatio": 9.0,
  "BaseRate": 4.25
}
```
The backend validates the four inputs, passes them through the saved scikit-learn pipeline and returns the predicted class together with model probabilities.

Example response:
```json
{
  "prediction": "Stretch",
  "probabilities": {
    "Achievable": 0.1307,
    "Stretch": 0.8544,
    "Unfeasible": 0.0149
  }
```
The frontend then presents the returned classification and probability information to the user.

## Architecture

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

