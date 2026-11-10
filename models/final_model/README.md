# Final Model Artifact

The trained XGBoost model is not bundled yet because the fitted `final_model` object exists in the Google Colab runtime after executing the notebook.

After the final model has been trained, run the following cell in Colab from the project root (adjust the destination if your folder differs):

```python
from pathlib import Path
import json

model_dir = Path("models/final_model")
model_dir.mkdir(parents=True, exist_ok=True)

final_model.save_model(model_dir / "xgboost_fraud_detector.json")

metadata = {
    "model": "XGBoost",
    "classification_threshold": 0.31,
    "target_mapping": {"0": "legitimate", "1": "fraud"},
    "feature_columns": list(X_train_combined.columns),
    "threshold_selection": "Selected using validation data; not tuned on final test data."
}

with open(model_dir / "model_metadata.json", "w", encoding="utf-8") as f:
    json.dump(metadata, f, indent=4)

print("Model and metadata saved.")
```

The final XGBoost model was trained on the original feature values, so a `StandardScaler` is not required for this final model. Keep the input feature names and order consistent during inference.

Before committing the model file, check its size and the repository hosting limits. The notebook and source code are the primary project artifacts.
