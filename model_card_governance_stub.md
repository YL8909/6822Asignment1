# Model Card and Governance Documentation Stub

## Prototype Name

Jurisdiction-Aware AI Credit Scoring Governance Prototype

## Submission Option

This Task 3 submission follows **Option A: Working Prototype**. The main implementation is `Task3_demo.ipynb`, supported by synthetic data, monitoring outputs, exported charts, and this governance documentation stub.

## Purpose

The prototype demonstrates how a RegTech tool could monitor an AI-supported credit scoring model across different regulatory jurisdictions. It is designed as a governance and compliance monitoring layer around a credit scoring model, not as a production lending system.

The tool monitors:

- Credit model performance
- Approval disparity
- False positive rate gap
- False negative rate gap
- Explainability through feature importance and rejection reasons
- Model drift during a simulated monitoring period
- Jurisdiction-specific compliance outcomes for the United States and European Union

## Intended Users

The intended users are:

- Compliance teams
- Model risk management teams
- Chief Compliance Officers
- AI governance committees
- Internal audit or risk review teams

The prototype is not intended for direct consumer use or direct automated lending decisions.

## Data Used

The prototype uses a synthetic loan dataset generated inside the notebook and exported as `synthetic_loan_data.csv`.

The dataset includes simulated applicant and loan attributes such as:

- Annual income
- Employment years
- Debt-to-income ratio
- Credit history years
- Missed payments in the past 12 months
- Prior default indicator
- Loan amount
- Product type
- Jurisdiction
- Protected group label for fairness testing

The dataset is synthetic. It does not contain real customer information, real HSBC data, or any real bank customer records.

## Model Type

The credit scoring model is a logistic regression classifier. Logistic regression was selected because it is simple, transparent, and suitable for a lightweight governance prototype.

The model predicts a repayment probability. The prototype then applies an internal approval threshold to generate a simulated approve/reject output.

## Features Used by the Model

The model uses:

- Age
- Annual income
- Employment years
- Debt-to-income ratio
- Credit history years
- Missed payments in the past 12 months
- Prior default indicator
- Loan amount
- Product type

## Sensitive or Governance Attributes

The `protected_group` attribute is used for fairness monitoring only. It is not used as an input feature for model training.

This design reflects a governance distinction:

- The model should not directly use protected group membership to score applicants.
- The governance layer still needs a protected group indicator in synthetic testing to detect potential disparate outcomes.

## Performance Metrics

The prototype reports:

- Accuracy
- AUC
- Confusion matrix

The latest exported performance summary is in `model_performance_summary.csv`.

## Fairness Metrics

The prototype monitors:

- Approval disparity
- False positive rate gap
- False negative rate gap

These metrics are exported in `fairness_metrics_summary.csv` and `fairness_group_rates.csv`.

The fairness metrics are governance indicators. They do not, by themselves, prove unlawful discrimination. A flagged result should trigger human review.

## Explainability Method

The prototype uses two lightweight explainability methods:

- Global feature importance from logistic regression coefficients
- Rule-based rejection reasons for rejected applicants

The rejection reasons are prototype explanation categories based on common credit risk factors, such as high debt-to-income ratio, recent missed payments, limited credit history, prior default, high requested loan amount relative to income, and short employment history.

These explanations are not final legal adverse action notices. They are designed to support review by compliance and model risk teams.

## Monitoring Plan

The prototype simulates six monitoring months after the baseline dataset. It calculates model performance, fairness metrics, approval rates, and model drift indicators during each month.

Model drift is monitored using Population Stability Index (PSI) for:

- Model score
- Annual income
- Debt-to-income ratio

The monitoring results are exported in `monitoring_results.csv`.

## Jurisdiction Configuration Layer

The jurisdiction configuration layer is implemented in the `RULES` dictionary inside `Task3_demo.ipynb`.

It stores jurisdiction-specific parameters for:

- Regulatory focus
- Legal sources
- Approval disparity threshold
- False positive rate gap threshold
- False negative rate gap threshold
- Drift threshold
- Adverse action explanation review
- Human oversight workflow
- High-risk AI documentation requirements

The prototype compares the United States and European Union.

## Regulatory Basis

The United States logic reflects fair lending and adverse action explanation requirements under:

- Equal Credit Opportunity Act (ECOA)
- Regulation B
- CFPB Circular 2022-03
- CFPB Circular 2023-03

The European Union logic reflects high-risk AI governance expectations under:

- Regulation (EU) 2024/1689, the EU AI Act

The prototype also references official regulatory source links in `Task3_demo.ipynb`.

## Internal Governance Thresholds

The numeric thresholds in the prototype are not statutory thresholds. They are internal governance thresholds selected to demonstrate how a compliance monitoring tool could operationalize real jurisdiction-specific regulatory expectations.

The prototype must not be read as claiming that US or EU law requires the exact numeric thresholds used in the notebook.

## Jurisdiction-Dependent Output

The prototype exports `jurisdiction_compliance_report.csv`, which shows how the same metrics can trigger different governance outcomes depending on the active jurisdiction.

For example, the same monitoring PSI value can trigger a drift-related high-risk AI governance response under the EU configuration while not triggering the same drift control under the US configuration. Fairness gaps can also trigger different governance responses, such as fair lending review in the United States and human oversight or documentation updates in the European Union.

## Human Review Requirements

Human review is required for:

- Final legal interpretation
- Review of adverse action explanation quality
- Determining whether a fairness alert indicates legally significant discrimination
- Deciding whether to retrain, suspend, or modify a model
- Resolving conflicting regulatory requirements
- Approving changes to jurisdiction rule parameters

The prototype supports human review. It does not replace it.

## Failure Modes

Important failure modes include:

- Synthetic data may not represent real lending populations.
- Internal thresholds may be misconfigured.
- A model may drift after deployment.
- Fairness metrics may miss forms of bias not captured by the protected group variable.
- Rejection reasons may oversimplify complex model behavior.
- A jurisdiction rule may change before the tool is updated.
- Two jurisdictions may require different governance responses for the same model output.

## Limitations

The prototype does not:

- Make final credit approval decisions
- Use real customer data
- Prove legal discrimination
- Replace legal, compliance, or model risk professionals
- Provide final adverse action notices to consumers
- Claim that its numeric thresholds are statutory requirements
- Provide production-grade security, audit logging, or rule versioning

## Future Improvements

Future versions could add:

- Formal rule versioning with effective dates and approval status
- A user interface for compliance teams
- SHAP or LIME explanations for more complex black-box models
- More detailed audit logs
- Expanded jurisdictions beyond the United States and European Union
- Real-world validation using approved, privacy-compliant lending datasets
