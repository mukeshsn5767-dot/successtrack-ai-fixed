# Report alignment and interface update

The source is the supplied 58-page `general(2).pdf`. The dashboard now provides a Report Figures view with twelve original report figures (3.1, 4.1–4.3, 5.1–5.8), extracted from the PDF rather than generated.

## Report reference values (historical pilot, not live overrides)
130 synthetic students: 78 low, 32 medium, 20 high; 52 review alerts. 5,850 attendance, 650 assessment, 1,300 LMS records. Average attendance 84.4%, average marks 77.58%, engagement 78.36%. Logistic regression and random forest five-fold CV accuracy 100%; XGBoost 96.1% CV accuracy and 94.3% macro F1. All three models 100% held-out test accuracy on 26 students. Selected classifier: logistic regression by tie-break.

**Important:** historical values are not forcibly displayed as live metrics. New imports and recomputations can change them. The synthetic pilot cannot substantiate accuracy on real students. Figures are historical report images.

## Run
Backend: install `backend/requirements.txt`, run `uvicorn app.main:app --reload` from `backend/`. Frontend: `cd frontend && npm ci && npm run dev`. For model re-training use `python -m ml.report_aligned` from the project root with ML dependencies installed. See original README for full setup.
