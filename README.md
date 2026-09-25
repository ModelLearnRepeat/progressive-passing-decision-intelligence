# Progressive Passing Decision Intelligence

A football analytics portfolio project using StatsBomb event and 360 data to evaluate progressive passing through attacking value, execution reliability, defensive context, and failure risk.

## Project Question

A progressive pass should not be judged only by how far it moves the ball forward.

The project asks:

> When does a progressive pass create enough attacking value to justify its execution and turnover risk?

The analysis follows a simple decision logic:

```text
Raw event data
-> Data cleaning and feature engineering
-> Progressive-pass KPI
-> Expected Threat model
-> Pass-level attacking value
-> Statistical reward-risk baseline
-> 360 defensive context
-> Same-destination comparison
-> Decision intelligence
```

## Executive Summary

The project analyses a 34-match Bundesliga 2023/24 sample.

Key figures:

| Metric | Result |
| --- | ---: |
| Total pass events | 39,214 |
| Dynamic-possession pass attempts | 19,397 |
| Progressive pass attempts | 3,342 |
| Completed progressive passes | 2,412 |
| Progressive completion rate | 72.2% |
| Non-progressive completion rate | 90.5% |
| Progressive mean Delta xT if completed | 0.0066 |
| Non-progressive mean Delta xT if completed | 0.0013 |

The main finding is a clear reward-risk trade-off:

- Progressive passes create more attacking value when they are completed.
- They are substantially harder to complete.
- Defensive context strongly affects execution reliability.
- Receiver space remains important even when destination area is held approximately constant.
- A useful decision framework should therefore combine reward, completion reliability, and failure exposure.

The same aggregate reward-risk direction appeared in all 34 analysed matches.

## Key Findings

### 1. Progressive passing creates more attacking value

Among completed passes, progressive passes produced a mean Delta xT of approximately 0.0066 compared with 0.0013 for non-progressive passes.

The difference is not driven only by a small number of extreme actions. The progressive-pass median is also clearly higher.

### 2. Progression comes with greater execution risk

Progressive-pass completion is approximately 72.2%, compared with 90.5% for non-progressive passes.

This is a difference of about 18.4 percentage points.

The project therefore keeps attacking reward and execution reliability separate instead of forcing them into a single KPI too early.

### 3. Receiver space strongly affects completion

Using StatsBomb 360 freeze-frame context, completion increases strongly as the receiver becomes more open.

In the full 360 sample, completion rises from approximately 58% in the tightest receiver-space group to approximately 86% in the most open group.

A stricter sensitivity analysis using only better receiver-location approximations preserves the same pattern.

### 4. Context still matters for similar destinations

To reduce the influence of destination value, the analysis compares passes within the same 12 x 8 destination states.

After destination standardisation:

- More receiver space is associated with an approximately 23.6 percentage-point completion advantage.
- Lower passer pressure is associated with an approximately 12.4 percentage-point completion advantage.

These are descriptive conditional comparisons, not causal effects.

### 5. Reward must be evaluated together with failure cost

The final decision layer combines:

- attacking reward when the pass succeeds
- completion probability
- lost attacking value after failure
- estimated opponent threat after failure

This produces a comparative reward-risk framework rather than a simple ranking based on progression volume or completion alone.

Within the analysed sample, the most favorable relative profile is associated with an open receiver and lower passer pressure. The least favorable relative profile is associated with a tightly covered receiver and high passer pressure.

The final metric is treated as a decision proxy, not as a complete causal possession-value model.

## Notebook Structure

### 01 - Data Foundation

File:

`notebooks/01_data_foundation.ipynb`

Purpose:

Build a clean and reproducible analytical dataset from raw StatsBomb event data.

Main tasks:

- Load the 34-match sample
- Extract pass events
- Separate dynamic possession from set pieces
- Engineer spatial and geometric features
- Define progressive pass attempts
- Create reusable processed datasets

Main skills demonstrated:

- Data cleaning
- Data transformation
- Feature engineering
- KPI definition
- Reproducible analysis pipelines

### 02 - Threat and Distributional Baseline

File:

`notebooks/02_threat_and_distributional_baseline.ipynb`

Purpose:

Estimate attacking value and establish the statistical reward-risk baseline.

Main tasks:

- Build a data-driven Expected Threat model
- Divide the pitch into a 12 x 8 grid
- Estimate shot and movement probabilities
- Estimate spatial transition probabilities
- Solve the xT system iteratively
- Calculate pass-level Delta xT
- Analyse the Delta xT distribution
- Compare progressive and non-progressive passes
- Use match-level robustness checks
- Quantify uncertainty with a match-cluster bootstrap

Main skills demonstrated:

- Statistical modeling
- Spatial state modeling
- Distribution analysis
- Estimation
- Bootstrap uncertainty
- Dependence-aware analysis

### 03 - Tactical Decision Intelligence

File:

`notebooks/03_tactical_decision_intelligence.ipynb`

Purpose:

Explain execution risk using defensive context and translate the results into a decision framework.

Main tasks:

- Link progressive passes with StatsBomb 360 data
- Measure receiver space
- Measure passer pressure
- Build packing and lane-density proxies
- Compare context and completion reliability
- Perform same-destination comparisons
- Build a failed-pass exposure proxy
- Combine reward and failure cost in a decision matrix
- Translate the results into decision implications

Main skills demonstrated:

- 360 spatial analytics
- Contextual segmentation
- Sensitivity analysis
- Conditional comparison
- Decision intelligence
- Risk-reward analysis

## Why This Project Is Relevant for Different Roles

### Business Intelligence

This project demonstrates:

- KPI design
- Segmentation
- Performance reporting
- Reward-risk comparison
- Executive interpretation
- Decision-focused metrics

### Data Analysis

This project demonstrates:

- Raw JSON data processing
- Data cleaning
- Feature engineering
- Exploratory analysis
- Distribution diagnostics
- Group comparisons
- Data quality checks

### Data Science

This project demonstrates:

- Expected Threat modeling
- Spatial state modeling
- Transition matrices
- Numerical convergence
- 360 feature engineering
- Cluster bootstrap analysis
- Context-based statistical comparisons

### Consulting

This project demonstrates:

- Problem structuring
- Breaking a broad KPI into decision components
- Separating reward from risk
- Identifying relevant trade-offs
- Translating technical analysis into business-style conclusions
- Communicating assumptions and limitations clearly

## Statistical Approach

The project follows the same logic throughout:

```text
Business or analytical question
-> Variable or model
-> Estimand
-> Estimator
-> Assumptions
-> Robustness or uncertainty
-> Decision interpretation
```

Important methodological choices include:

- Raw Delta xT observations are not assumed to be normally distributed.
- Mean, median, quantiles, and ECDFs are used together to describe the outcome distribution.
- Passes within the same match are not treated as fully independent observations.
- Match-cluster bootstrap resampling is used for the main aggregate uncertainty analysis.
- Same-destination comparisons are used to reduce spatial confounding.
- Results are described as observational and model-based rather than causal.

## Repository Structure

```text
progressive-passing-decision-intelligence/
|
|-- README.md
|-- requirements.txt
|-- config.example.json
|
|-- notebooks/
|   |-- 01_data_foundation.ipynb
|   |-- 02_threat_and_distributional_baseline.ipynb
|   `-- 03_tactical_decision_intelligence.ipynb
|
|-- figures/
`-- processed/
```

## How to Review the Project

If you have only a few minutes:

1. Read the Executive Summary and Key Findings in this README.
2. Open Notebook 03 for the final decision-intelligence analysis.

For a specific role:

| Role | Start here |
| --- | --- |
| Business Intelligence | Notebook 03 |
| Consulting | Notebook 03 |
| Data Science | Notebook 02 |
| Statistics | Notebook 02 |
| Data Analysis | Notebook 01 |
| Data Engineering | Notebook 01 |

## Reproducibility

Run the notebooks in this order:

```text
01_data_foundation.ipynb
02_threat_and_distributional_baseline.ipynb
03_tactical_decision_intelligence.ipynb
```

Install the required packages:

```bash
pip install -r requirements.txt
```

Create a local `config.json` in the repository root:

```json
{
  "statsbomb_data_dir": "PATH/TO/STATSBOMB/OPEN-DATA/data"
}
```

The local `config.json` should not be committed because it contains a machine-specific path.

The project uses StatsBomb Open Data:

https://github.com/statsbomb/open-data

## Limitations

- The analysis covers 34 matches and is not treated as a representative league-wide Bundesliga estimate.
- The results are observational and model-based, not causal.
- StatsBomb 360 information is not available for every progressive pass.
- Receiver location is approximated from freeze-frame geometry and is therefore treated as a proxy.
- Failed-pass exposure is a constructed decision proxy rather than an observed causal turnover cost.
- The xT surface is estimated from the analysed sample and then treated as fixed in downstream calculations.
- The final net-value measure is intended for relative comparison between context profiles, not as an absolute measure of strategic value.

## Final Takeaway

Progressive passing is not only a question of moving the ball forward.

Its decision value depends on three separate dimensions:

- attacking reward
- execution reliability
- downside after failure

This project turns those dimensions into a transparent and reproducible decision-intelligence framework.
