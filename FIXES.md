# Bug-fix log (behaviour only - UI layout/styling unchanged)

## Backend
1. **Data loss on restart** - `seed_database_from_csv` wiped the whole DB at every start unless it held exactly 130 students (so any CSV upload was lost). It now only seeds an empty/legacy DB (or when `force=True`).
2. **Invalid rows were imported** - ingestion validated rows but then imported *all* of them (unknown students, bad statuses, negative marks, bad dates). Only validated rows are written now.
3. **`student_dataset` upload destroyed the database** (it re-seeded from `data/raw`). It now updates cohort demographics (Gender/Program/YearLevel/ClassTime) for existing students only.
4. Student-id aliasing no longer maps a *new* roster row onto a different existing student; duplicate emails are rejected instead of causing a 500.
5. CSV parsing tolerates Excel BOM, `1001.0` ids, blank cells (previously stored the text "nan"); dates are validated.
6. Legacy `ml.train_models` / `batch_scorer` (needs `shap`, old 11-feature schema) is no longer imported at start-up; ~300 lines of dead legacy pipeline removed.
7. `init_db` migrations used SQLite-only `PRAGMA` (silently failed on PostgreSQL) - now dialect-agnostic.
8. Risk explanation: `impact` sign was inverted relative to the UI (low attendance showed as *protective*), features on different scales were ranked by raw size, and "primary risk driver" could be a favourable feature. Impact is now a scale-normalised signed contribution to risk, risk drivers first.
9. Scoring aligns probabilities by `model.classes_`; XGBoost import is optional.
10. Cohort analytics showed `0%` predicted grade and mislabelled values (attendance in %, shown as "/10"; quiz/midterm/LMS read from legacy names). Fixed and computed with single queries instead of N+1.
11. `/recompute?retrain=true` scored twice; errors now return clean 4xx/5xx JSON. Start-up no longer crashes if seeding fails.
12. Intervention endpoints validate `follow_up_date` (YYYY-MM-DD) and staff; CSV report neutralises spreadsheet-formula injection and sanitises the filename; CORS no longer combines credentials with wildcard.
13. `start_local.py` trained the wrong (legacy) model and only worked on Windows.

## Frontend (logic only)
- Demo fallback: duplicate student ids, historical-analytics data in the wrong shape (section rendered empty), `||` dropping zero values, missing fields.
- A real API 404 for an unknown student no longer silently shows a different (demo) student.
- Student dossier clears stale profile/error when switching students.
- Hard-coded "192 students", fake fallback R²/accuracy and the empty regression table removed; retrain message reports real numbers.
- Registry sort no longer crashes on null text values.

## Tests
Predict endpoints now asserted as explicitly unsupported (422); new tests for restart persistence, valid-rows-only ingestion, 404 and date validation.
