# Viva alignment: 130-student pilot

This copy uses the 130-student `data/raw/` cohort, the twelve features from the printed report, and a 3-class academic-risk classifier. `ml/report_aligned.py` trains three candidates with stratified 80/20 split and five-fold CV, selects highest held-out macro F1 (tie order Logistic Regression, Random Forest, XGBoost), and persists the actual model and metrics. It uses the report risk score `0.45 * P(Medium) + P(High)` and the original `ml/feature_engineering.py` 35/30/20/15 engagement weights.

Run from the project root:

```bash
pip install -r backend/requirements.txt
python -m ml.report_aligned
uvicorn backend.app.main:app --reload
```

The backend will replace an existing legacy 192-student database with the pilot cohort on startup. **Back up any existing database before running**. Delete `classroom_analytics.db` if you want a fresh start. API: `GET /api/model/info`, `GET /api/class/CS101/risk-summary`, `POST /api/recompute`, `POST /api/seed`. The frontend remains the original React app; run it according to `frontend/README.md`.

**Important caveats:** The printed report's exact 100% results are not guaranteed; the actual model metadata contains reproduced measurements. The selected model is whatever the stated rule produces. The existing dashboard contains legacy predicted-grade displays, which are not meaningful for a classification-only model and may appear blank. SHAP is not claimed for these predictions; the model's factors are explicitly labeled heuristic baseline deviations. Historical snapshots are not fabricated. The legacy 192-student training code remains in `ml/train_models.py` for reference but is not the report-aligned pipeline. Uploaded data must contain meaningful attendance, assessment and LMS records to support inference. Model generalization to real student data is unverified.
