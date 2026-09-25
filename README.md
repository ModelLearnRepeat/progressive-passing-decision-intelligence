# Progressive Passing Under Risk  
## From Football Event Data to Tactical Decision Intelligence

**Python â€¢ Statistical Analysis â€¢ Spatial Analytics â€¢ KPI Design â€¢ Decision Intelligence**

This portfolio project turns StatsBomb event data and 360 freeze-frame context into a decision-support framework for one practical question:

> **When does a progressive pass create enough attacking value to justify its execution and turnover risk?**

The project is designed to demonstrate skills relevant to **Business Intelligence, Data Analysis, Data Science, and Consulting**: building a reproducible data pipeline, defining decision-relevant KPIs, estimating attacking value, quantifying uncertainty, testing contextual explanations, and translating technical results into clear business-style recommendations.

---

## Recruiter Quick View

| Area | What this project demonstrates |
|---|---|
| **Business Intelligence** | KPI design, segmentation, reward-risk reporting, decision matrix, executive interpretation |
| **Data Analysis** | Data cleaning, feature engineering, exploratory distribution analysis, contextual comparisons |
| **Data Science** | Data-driven xT model, spatial state modeling, 360-derived features, bootstrap uncertainty |
| **Consulting** | Problem structuring, trade-off analysis, decision framework, limitations, actionable conclusions |

### Project scope

- **34 Bundesliga 2023/24 matches**
- **39,214 total pass events**
- **19,397 dynamic-possession pass attempts**
- **3,342 progressive pass attempts**
- **2,614 progressive attempts with StatsBomb 360 context**
- Reproducible three-notebook workflow from raw events to decision intelligence

---

# Executive Summary

Progressive passing creates a clear **reward-risk trade-off**.

Completed progressive passes generated substantially more model-based attacking value than completed non-progressive passes, but they were also much harder to complete.

| KPI | Progressive | Non-progressive |
|---|---:|---:|
| Attempts | 3,342 | 16,055 |
| Completion rate | **72.2%** | **90.5%** |
| Failure rate | **27.8%** | **9.5%** |
| Mean Î”xT if completed | **0.0066** | **0.0013** |
| Median Î”xT if completed | **0.0049** | **0.0000** |
| Positive Î”xT if completed | **97.2%** | **46.4%** |

The observed mean reward difference was **+0.00532 Î”xT**, while the completion-rate difference was **âˆ’18.37 percentage points**.

A match-cluster bootstrap produced:

- reward-difference interval: **[0.00476, 0.00584]**
- completion-rate-difference interval: **[âˆ’20.17, âˆ’16.58] percentage points**

The same reward-risk direction appeared in **all 34 analysed matches**.

The final 360 analysis adds an important layer: **execution context matters even when destination is held approximately constant**.

- More receiver space was associated with an approximately **+23.6 percentage-point** completion advantage after destination standardisation.
- Lower passer pressure was associated with an approximately **+12.4 percentage-point** completion advantage.
- The receiver-space pattern remained strong in a stricter sensitivity analysis using only passes with a receiver-location proxy error of at most 10 coordinate units.

The final decision framework therefore separates three dimensions:

> **attacking reward + execution reliability + failure exposure**

rather than evaluating progressive passing through volume or completion alone.

---

# Business Question

A standard progressive-pass KPI answers:

> **Did the ball move forward?**

This project asks more useful decision questions:

1. **How much attacking value does successful progression create?**
2. **How reliable is the action?**
3. **How does defensive context affect execution?**
4. **Does context still matter for similar destination areas?**
5. **What downside is created when the action fails?**
6. **Which tactical situations provide the most attractive relative reward-risk profile?**

---

# Analytical Workflow

```text
Raw StatsBomb event data
        â†“
Data cleaning and possession scope
        â†“
Geometric progression KPI
        â†“
Data-driven Expected Threat model
        â†“
Pass-level Î”xT
        â†“
Distributional + cluster-aware statistical analysis
        â†“
StatsBomb 360 defensive-context features
        â†“
Same-destination contextual comparison
        â†“
Reward + reliability + failure-cost framework
        â†“
Decision intelligence
```

---

# Notebook Guide

## 01 â€” Data Foundation & KPI Engineering

[`01_data_foundation.ipynb`](notebooks/01_data_foundation.ipynb)

**Purpose:** transform raw StatsBomb events into a clean analytical pass dataset and define the progression KPI.

### Main steps

- loads the 34-match Bundesliga sample
- extracts **39,214 pass events**
- separates dynamic possession from set-piece phases
- engineers:
  - pass length
  - forward distance
  - pass angle
  - pass direction
  - pitch thirds
  - lateral lanes
  - zone transitions
- defines progressive pass attempts using spatial advancement
- exports reusable processed datasets for downstream analysis

### Initial KPI result

Progressive attempts represent approximately **17.2%** of dynamic-possession passes.

Their completion rate is **72.2%**, compared with **90.5%** for non-progressive passes.

This establishes the first business problem:

> **Progression offers attacking upside, but comes with substantially greater execution risk.**

---

## 02 â€” Threat and Distributional Baseline

[`02_threat_and_distributional_baseline.ipynb`](notebooks/02_threat_and_distributional_baseline.ipynb)

**Purpose:** estimate attacking value and establish the statistical reward-risk baseline.

### Data-driven Expected Threat model

The pitch is divided into a **12 Ã— 8 grid with 96 spatial states**.

For each state, the model estimates:

- probability of taking a shot
- probability of a successful movement
- probability of a failed movement
- expected shot value
- transition probabilities between spatial states

The recursive xT system converged after **87 iterations**.

The resulting state values range from approximately:

- **minimum xT: 0.0060**
- **maximum xT: 0.2688**

The final fixed-point residual is below \(10^{-6}\), providing a numerical validation of convergence.

### Pass-level attacking value

For a completed pass:

$$
\Delta xT = xT_{\text{end}} - xT_{\text{start}}
$$

Positive Î”xT means the pass moves possession to a state with greater estimated future attacking value.

### Why distribution matters

The empirical Î”xT distribution is strongly asymmetric:

- mean: **0.00208**
- median: **0.00055**
- skewness: **9.03**
- excess kurtosis: **123.20**

Because the raw outcome distribution is highly concentrated around zero and strongly asymmetric, the analysis does not impose a simple Normal model on individual Î”xT observations.

Instead, the notebook combines:

- means
- medians
- quantiles
- ECDFs
- IQR-based diagnostics
- match-level comparisons
- match-cluster bootstrap uncertainty

### Reward-risk baseline

Among completed passes:

- progressive mean Î”xT: **0.00664**
- non-progressive mean Î”xT: **0.00132**
- progressive median Î”xT: **0.00492**
- non-progressive median Î”xT: **0.00000**

At the same time:

- progressive completion: **72.17%**
- non-progressive completion: **90.54%**

The notebook therefore keeps **reward** and **execution reliability** separate instead of collapsing them prematurely into one score.

---

## 03 â€” Tactical Decision Intelligence

[`03_tactical_decision_intelligence.ipynb`](notebooks/03_tactical_decision_intelligence.ipynb)

**Purpose:** move from aggregate progression metrics to context-aware decision analysis.

### 360 data availability

Of the 3,342 progressive attempts:

- **2,614 (78.2%)** have StatsBomb 360 context
- completion among 360-covered attempts: **72.9%**

A coverage check shows only a modest completion difference between passes with and without 360 information, while conditional attacking reward is similar.

### Defensive and tactical context

The notebook derives interpretable context features from 360 freeze frames:

- **receiver space**
- **passer pressure**
- **packing proxy**
- **passing-lane density**
- **line-breaking / penetration proxies**

The strongest descriptive pattern is receiver space.

Across the full 360 sample, completion rises from approximately:

- **58.0%** in the tightest receiver-space quartile
- to **86.4%** in the most open quartile

A stricter receiver-location sensitivity analysis retains the same direction:

- **71.1%** completion in the tightest-space group
- **96.0%** in the most-open group

### Same destination, different context

To reduce the influence of pass destination, the analysis compares context conditions **within the same 12 Ã— 8 destination states**.

After destination standardisation:

| Context comparison | Completion advantage |
|---|---:|
| More receiver space | **+23.6 pp** |
| Lower passer pressure | **+12.4 pp** |

More receiver space produced higher completion in **23 of 28** eligible destination states.

Lower passer pressure produced higher completion in **24 of 33** eligible destination states.

These are **descriptive conditional comparisons, not causal estimates**.

### Decision intelligence

The final layer combines:

1. **expected attacking reward**
2. **execution reliability**
3. **failed-pass cost proxy**

The failure-cost proxy includes:

- attacking state value lost when possession ends
- estimated opponent threat associated with the failed-pass location

This creates a comparative reward-risk framework across four context profiles:

- open receiver / low passer pressure
- open receiver / high passer pressure
- tight receiver / low passer pressure
- tight receiver / high passer pressure

Within this sample, **open receiver / low passer pressure** provides the most favorable relative balance between attacking reward and failure cost, while **tight receiver / high passer pressure** produces the least favorable relative profile.

The resulting metric is intentionally treated as a **decision proxy**, not as a causal or complete possession-value model.

---

# Statistical Design

The project follows a consistent statistical logic:

```text
Variable / model
      â†“
Estimand
      â†“
Estimator
      â†“
Assumptions
      â†“
Uncertainty / robustness
      â†“
Decision interpretation
```

Examples include:

### Completion probability

$$
p_g = P(C=1 \mid G=g)
$$

estimated by the empirical completion proportion.

### Conditional attacking reward

$$
\mu_g
=
E[\Delta xT \mid C=1, G=g]
$$

estimated by the pass-weighted sample mean among completed passes.

### Cluster-aware uncertainty

Passes within the same match are not treated as fully independent sampling units.

The primary aggregate contrasts therefore use a **match-cluster bootstrap**, resampling complete matches and recalculating the same pass-weighted estimators.

### Destination-standardised comparison

Context comparisons are repeated within the same destination states before aggregation, reducingâ€”but not eliminatingâ€”spatial confounding.

---

# What I Would Show in a BI / Consulting Interview

### 1. KPI design

A simple progressive-pass count is not enough.

A useful reporting layer should separate:

- **usage**
- **completion reliability**
- **attacking reward**
- **failure exposure**

### 2. Segmentation before recommendation

Aggregate averages hide important contextual differences.

Receiver availability, congestion and pressure materially change the execution profile of a progressive action.

### 3. Reward and risk should not be collapsed too early

A high-value action may also have a high failure probability.

The project therefore evaluates upside and downside separately before constructing a final comparative decision proxy.

### 4. Statistical uncertainty matters

The project explicitly addresses:

- non-Normal outcome distributions
- within-match dependence
- cluster-aware uncertainty
- 360 coverage
- proxy sensitivity
- contextual confounding
- limits of causal interpretation

---

# Skills Demonstrated

### Business Intelligence

- KPI architecture
- business-question framing
- segmentation
- executive tables
- reward-risk matrix
- decision-oriented reporting

### Data Analysis

- JSON event-data ingestion
- cleaning and transformation
- feature engineering
- exploratory data analysis
- grouped comparisons
- distribution diagnostics
- data-quality checks

### Data Science

- spatial state modeling
- recursive Expected Threat estimation
- transition matrices
- numerical convergence validation
- 360 freeze-frame feature engineering
- bootstrap uncertainty
- conditional and standardised comparisons

### Consulting

- structured problem decomposition
- hypothesis-driven analysis
- trade-off evaluation
- prioritisation of decision-relevant metrics
- translation of statistical findings into recommendations
- explicit treatment of assumptions and limitations

---

# Repository Structure

```text
football-performance-analytics/
â”‚
â”œâ”€â”€ README.md
â”œâ”€â”€ requirements.txt
â”œâ”€â”€ config.json                  # local only; not committed
â”‚
â”œâ”€â”€ notebooks/
â”‚   â”œâ”€â”€ 01_data_foundation.ipynb
â”‚   â”œâ”€â”€ 02_threat_and_distributional_baseline.ipynb
â”‚   â””â”€â”€ 03_tactical_decision_intelligence.ipynb
â”‚
â”œâ”€â”€ processed/                   # generated analytical outputs
â””â”€â”€ figures/                     # optional exported portfolio visuals
```

---

# Reproducibility

The notebooks are designed to run in order:

```text
01 â†’ 02 â†’ 03
```

Install the required Python packages:

```bash
pip install -r requirements.txt
```

Create a local `config.json` in the repository root:

```json
{
  "statsbomb_data_dir": "PATH/TO/STATSBOMB/OPEN-DATA/data"
}
```

`config.json` should remain excluded from Git because it contains a machine-specific local path.

The analysis uses **StatsBomb Open Data**:

https://github.com/statsbomb/open-data

---

# Key Limitations

This project is intentionally explicit about its scope.

- The analysis covers **34 matches**, not a random representative sample of the entire Bundesliga.
- Results are **descriptive and model-based**, not causal effects.
- StatsBomb 360 context is available for approximately **78%** of progressive attempts.
- Receiver identity is approximated from freeze-frame geometry and is therefore treated as a proxy.
- Failed-pass exposure is a constructed decision proxy rather than an observed causal turnover cost.
- The xT surface is estimated from the analysed sample and treated as fixed in downstream calculations; xT-estimation uncertainty is not propagated through every later statistic.
- Progression and xT are both related to spatial advancement, so the aggregate progressive-versus-non-progressive comparison is treated as a baseline rather than the final tactical conclusion.

---

# How to Review This Project

**If you have 2 minutes:**  
Read the **Executive Summary** and **Business Question** above.

**If you are interested in Business Intelligence or Consulting:**  
Start with [`03_tactical_decision_intelligence.ipynb`](notebooks/03_tactical_decision_intelligence.ipynb).

**If you are interested in Data Science or Statistics:**  
Start with [`02_threat_and_distributional_baseline.ipynb`](notebooks/02_threat_and_distributional_baseline.ipynb).

**If you are interested in Data Engineering / Data Analysis:**  
Start with [`01_data_foundation.ipynb`](notebooks/01_data_foundation.ipynb).

---

## Final Takeaway

> **Progressive passing is not simply a question of moving the ball forward.  
> The decision value depends on the attacking reward created, the probability of successful execution, and the downside of losing possession under the observed tactical context.**

This project turns that trade-off into a transparent, reproducible analytical framework.
