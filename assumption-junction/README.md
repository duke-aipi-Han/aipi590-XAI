# Name: Han Wang

## Dataset

The "Telco Customer Churn" dataset contains:
* rows of customers
* whether they churned or not (in the past month)
* their attributes such as services, account info, and charges/payments
* and basic demographics data

Some columns that have demographics data that may produce discriminatory results were excluded (such as gender, senior, have partner or have children)

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression | Linearity, independent observations, constant variance, residual normality, multicollinearity | Residual plots, residual histogram, correlation heatmap | Binary outcome produces non-normal and heteroscedastic residuals; use as a baseline only |
| Logistic regression | Binary outcome, independent observations, finite predictors, calibration, threshold behavior | Churn encoding, stratified split, numerical checks, calibration curve, threshold table | One split limits generalization; predictive associations are not causal effects |
| GAM | Binary outcome, independent observations, additive structure, smooth effects, predictor redundancy | Logistic GAM, smooth-effect plots, correlation heatmap, ROC-AUC and calibration | `tenure` and `TotalCharges` are strongly related; smoothness and validation need further tuning |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression | Lowest ROC-AUC among the primary models; included as a baseline | Very simple coefficients | Not designed for binary probabilities; assumptions are not well supported |
| Logistic regression | ROC-AUC, precision, recall, F1, Brier score, and threshold results | Coefficients are easier to communicate | Many non-obvious and not high correlations in features |
| GAM | Highest ROC-AUC and lowest Brier score in the clean test split | Smooth plots show nonlinear relationships | More complex and more sensitive to smoothing and predictor redundancy |

## Recommendation

Recommended model: Logistic regression for the initial operational model, with GAM as a nonlinear sensitivity analysis.

Why this model: The GAM had marginally better ranking and calibration in the test split, but logistic regression had slightly higher churn recall at the default threshold and is much simpler to explain and deploy. It is also a "well known" model. Threshold selection could be tuned and should reflect the cost of missing a potential churner.

What the company can responsibly conclude: The models can rank customers by estimated churn risk. Contract type, internet service, tenure, and support-related variables are associated with different predicted churn risks in this dataset.

What the company should not conclude yet: These coefficients do not prove that changing a service or contract will cause a customer to stay. Performance is based on one test split and should be confirmed with cross-validation and prospective evaluation. Excluded factors that might lead to discrimination should be discarded early in process.

One next analysis we would run: Evaluate the selected model with stratified cross-validation and choose a probability threshold using the business cost of false negatives versus false positives.

## Reproducibility Notes

The notebook requires Python packages listed in the project requirements, and most are already available in Colab; Non-colab requirements are listed in requirements-collab.txt

KaggleHub must be able to download the Telco Customer Churn dataset. 