# Transportation Analytics Dashboard — Exploratory Data Analysis & Pattern Discovery Report
**Project Milestone**: Step 04 — Exploratory Data Analysis (EDA)  
**Author**: Senior Transportation Data Analyst  
**Dataset Analyzed**: `data/processed/transportation_cleaned.parquet` / `transportation_cleaned.csv`  
**Temporal Coverage**: January 1, 2018 – December 28, 2023 (6 Continuous Calendar Years)  
**Status**: Completed & Evidence-Backed

---

## 1. Executive Summary & Dataset Architecture

This Exploratory Data Analysis (EDA) report discovers operational patterns, safety anomalies, temporal trends, and infrastructure risks across the cleaned Indian highway and corridor telemetry dataset. 

Every observation, statistic, and strategic question in this report is directly grounded in the cleaned dataset ($N = 5,500$ records, 35 attributes) without fabricated financial numbers or unsupported assumptions.

### Dataset Profile Summary
* **Total Observations**: **5,500** telemetry recordings.
* **Corridor Coverage**: **500** distinct highway corridors (`RD-1` through `RD-500`) distributed across 5 Indian geographic zones.
* **Analytical Dimensions**: **16** categorical/hierarchical dimensions.
* **Continuous Measures**: **11** physical, flow, and incident metrics.
* **Time Span**: **72 calendar months** (2018–2023).
* **Base Collision Count**: **575** verified accidents (**10.45%** base incidence rate).
* **Total Vehicle Volume Monitored**: **1,397,508** vehicles.

---

## 2. Business Structure: Dimension & Measure Catalog

In strict accordance with analytics engineering standards, the 35 attributes of the cleaned dataset are formally cataloged into their operational roles:

| Category | Columns | Analytical Function / Role |
| :--- | :--- | :--- |
| **Identifiers** | `record_id`, `road_id` | Primary surrogate key and physical corridor segment tracking |
| **Dates / Timestamps** | `timestamp`, `year`, `quarter`, `month`, `month_name`, `day`, `day_of_week`, `hour` | Continuous time-series and seasonal aggregation anchors |
| **Analytical Dimensions** | `region`, `vehicle_type`, `traffic_density`, `weather`, `road_condition`, `accident_occurred`, `accident_severity`, `alert_generated`, `alert_success_type`, `visibility_category`, `time_of_day`, `is_weekend` | Multi-dimensional slicing, cohort grouping, and filtering |
| **Continuous Measures** | `vehicle_count`, `avg_speed_kmh`, `traffic_flow_rate`, `visibility_m`, `temperature_c`, `humidity_pct` | Additive, averaging, and statistical measure computation |
| **Binary Safety Measures** | `accident_flag`, `alert_flag`, `severe_accident_flag`, `fatal_accident_flag`, `traffic_density_score` | Vectorized risk calculation, incident sums, and rate modeling |

### Scope Clarification on Retail vs. Transportation Data
* **Present Telemetry**: Vehicle counts, speeds, modal types, congestion density, meteorology, road surface states, collisions, severity, and hazard alerts.
* **Absent Retail Ledger Fields**: Revenue, retail sales price, customer ID, product SKU, delivery dispatch deadlines, and fuel burn.
* **Analytical Strategy**: In accordance with project instructions (*"Do not force customer or product analysis if these dimensions are not present"*), this report focuses on the true currency of transportation networks: **traffic throughput, corridor velocity, collision risk, and warning alert reliability**.

---

## 3. Descriptive Business Analysis: Operational Measures

Summary metrics computed across all physical telemetry measures:

| Operational Metric | Total Network Volume | Mean $\pm$ Std Dev | Median (P50) | IQR ($Q_3 - Q_1$) | Observed Range [Min – Max] | Operational Meaning |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Vehicle Volume** (`vehicle_count`) | **1,397,508** | $254.1 \pm 141.4$ | 255.0 | 246.3 | 10 – 499 vehicles | Traffic intensity across monitored intervals |
| **Network Speed** (`avg_speed_kmh`) | — | $61.3 \pm 33.2$ | 60.0 | 57.0 | 5 – 119 km/h | Free-flow velocity across highway segments |
| **Traffic Flow Rate** (`traffic_flow_rate`) | **85,570,979** | $15,558.4 \pm 12,042.8$ | 12,688.0 | 17,329.0 | 80 – 59,262 | Vehicle-velocity momentum (flow intensity) |
| **Optical Visibility** (`visibility_m`) | — | $5,046.9 \pm 2,806.2$ | 5,007.0 | 4,875.0 | 201 – 9,998 m | Atmospheric sightline distance |
| **Temperature** (`temperature_c`) | — | $34.9 \pm 8.7$ | 34.9 | 15.0 | 20.0 – 50.0 °C | Ambient roadway heat stress |
| **Humidity** (`humidity_pct`) | — | $59.6 \pm 17.2$ | 59.5 | 29.5 | 30.0 – 90.0 % | Atmospheric moisture |

---

## 4. Transportation Volume Analysis

### A. Regional Volume Breakdown
| Region | Monitored Observations | Total Vehicle Volume | Volume Share (%) | Mean Vehicles / Obs | Mean Speed (km/h) | Accident Rate (%) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **North** | 1,725 | **436,191** | **31.21%** | 252.9 | 61.3 | **11.36%** |
| **South** | 1,480 | **367,747** | **26.31%** | 248.5 | 61.4 | **11.08%** |
| **East** | 927 | **241,835** | **17.30%** | 260.9 | 61.2 | **9.39%** |
| **Central** | 805 | **208,215** | **14.90%** | 258.7 | 61.1 | **9.07%** |
| **West** | 563 | **143,520** | **10.27%** | 254.9 | 61.6 | **9.77%** |
| **Total** | **5,500** | **1,397,508** | **100.0%** | **254.1** | **61.3** | **10.45%** |

* **Volume Finding**: North and South India collectively account for **57.5% of total national monitored volume** and also exhibit the highest collision rates ($11.36\%$ and $11.08\%$).

### B. Modal Vehicle Class Activity
| Vehicle Class | Observations | Total Volume Handled | Volume Share (%) | Mean Speed (km/h) | Collision Rate (%) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Truck** | 1,058 | 272,079 | 19.47% | 61.9 | 9.64% |
| **Car** | 1,172 | 299,650 | 21.44% | 60.2 | 10.92% |
| **Mixed** | 1,096 | 279,868 | 20.03% | 61.2 | 10.31% |
| **Bus** | 1,103 | 279,387 | 19.99% | 60.5 | 10.79% |
| **Bike** | 1,071 | 266,524 | 19.07% | 63.0 | 10.55% |

* **Modal Finding**: Traffic activity is evenly balanced across vehicle types (~20% each). Bikes record the highest average speed ($63.0\text{ km/h}$), while Cars have the highest collision rate ($10.92\%$).

---

## 5. Corridor Safety & High-Risk Anomaly Ranking

Analysis of individual corridor segments (`road_id`) revealed sharp localized risk concentrations:

| Corridor Identifier | Telemetry Observations | Total Accidents | Fatal Crashes | Severe Crashes (Fatal+Major) | Corridor Accident Rate (%) | Fatality Rate (%) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`RD-137`** | 9 | 4 | **3** | 4 | **44.44%** | **33.33%** |
| **`RD-170`** | 23 | **5** | 2 | 3 | 21.74% | 8.70% |
| **`RD-136`** | 12 | **5** | 0 | 2 | **41.67%** | 0.00% |
| **`RD-400`** | 17 | 4 | 2 | 2 | 23.53% | 11.76% |
| **`RD-450`** | 15 | 4 | 0 | 1 | 26.67% | 0.00% |
| **`RD-192`** | 11 | 4 | 1 | 2 | 36.36% | 9.09% |
| **`RD-81`** | 12 | 4 | 1 | 2 | 33.33% | 8.33% |
| **`RD-208`** | 12 | 4 | 1 | 2 | 33.33% | 8.33% |

### Critical Anomaly: `RD-137`
* `RD-137` has an unprecedented **44.44% accident rate**, with **3 fatal collisions out of only 9 observations**. This represents a severe localized physical hazard (e.g. blind corner, sharp grade, or deteriorated maintenance) requiring immediate engineering intervention.

---

## 6. Hazard Warning System Evaluation

A rigorous confusion matrix audit was performed comparing `alert_generated` against actual `accident_occurred`:

```text
                     Actual Collision: NO    Actual Collision: YES      Total
Alert Dispatched: NO        3,929 (TN)               468 (FN)           4,397
Alert Dispatched: YES         996 (FP)               107 (TP)           1,103
Total                       4,925                    575                5,500
```

### System Performance Metrics
* **Alert Sensitivity (Recall)**: **18.61%** (Only 107 of 575 collisions were preceded by an alert).
* **Missed Incident Rate (False Negative Rate)**: **81.39%** (468 collisions occurred with zero warning).
* **False Alarm Rate (1 - Precision)**: **90.30%** (996 out of 1,103 dispatched alerts had no incident).
* **System Precision**: **9.70%** (Fewer than 1 in 10 alerts corresponds to an actual collision).
* **Specificity**: **79.78%** (3,929 of 4,925 safe periods correctly unalerted).

### Strategic Diagnostic
> **Core Finding**: The current automated safety alert algorithm exhibits **severe alarm fatigue** ($90.3\%$ false positives) while failing to catch over **$81\%$ of actual crashes**. Re-engineering this warning mechanism via multi-factor machine learning (combining density, visibility, speed, and road condition) is a top strategic priority.

---

## 7. Time-Series & Daypart Analysis

### A. Operational Daypart Patterns
| Daypart Window | Hours | Vehicle Volume | Volume Share (%) | Mean Speed (km/h) | Collisions | Accident Rate (%) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Morning Peak** | 07:00 – 10:59 | 230,866 | 16.52% | **63.5** | 93 | 10.31% |
| **Midday** | 11:00 – 16:59 | 347,495 | 24.87% | 59.5 | 128 | 9.34% |
| **Evening Peak** | 17:00 – 21:59 | 302,647 | 21.66% | 60.7 | 118 | 9.83% |
| **Night** | 22:00 – 06:59 | **516,500** | **36.96%** | 62.0 | **236** | **11.64%** |

* **Nighttime Safety Risk**: Nighttime operations account for **36.96% of volume and 41.04% of all collisions** (236 crashes), recording the highest crash probability ($11.64\%$). Nighttime speed remains elevated ($62.0\text{ km/h}$), exacerbating crash severity.

### B. Multi-Year Trajectory (2018–2023)
| Year | Telemetry Observations | Total Volume | Mean Speed (km/h) | Total Collisions | Fatal Crashes | Alerts Generated |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **2018** | 905 | 233,779 | 61.2 | 99 | 24 | 188 |
| **2019** | 907 | 229,400 | 61.6 | 98 | 27 | 180 |
| **2020** | 860 | 221,024 | 60.7 | **77** | **17** | 190 |
| **2021** | 949 | 242,981 | 61.4 | **107** | 27 | 202 |
| **2022** | 954 | 237,274 | 61.6 | 98 | 26 | 161 |
| **2023** | 925 | 233,050 | 61.4 | 96 | 26 | 182 |

* **Macro Trend Finding**: Total volume and crashes remained stable across the 6-year period, with a distinct **2020 mobility dip** (volume dropped to $221\text{k}$, collisions dropped to $77$), reflecting synthetic modeling of pandemic travel restrictions, followed by a rebound in 2021 ($107$ crashes).

---

## 8. Environmental & Atmospheric Risk Analysis

Evaluation of weather states and pavement conditions:

| Weather Condition | Observations | Mean Visibility (m) | Mean Speed (km/h) | Collisions | Collision Rate (%) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Clear** | 1,073 | 5,173.0 | 61.3 | 114 | 10.62% |
| **Snow** | 1,091 | 5,137.4 | 61.3 | 110 | 10.08% |
| **Rain** | 1,144 | 4,997.1 | 61.2 | 124 | 10.84% |
| **Fog** | 1,099 | 5,016.5 | 61.2 | 116 | 10.56% |
| **Storm** | 1,093 | 4,915.6 | 61.6 | 111 | 10.16% |

* **Weather Finding**: Pavement condition and weather types are synthetically modeled with near-uniform distribution (~20% each). Crash rates remain consistently between $10.0\%$ and $10.8\%$ across all atmospheric conditions.

---

## 9. Correlation & Outlier Investigation

### Correlation Findings
* Raw pairwise correlations between independent physical features (`avg_speed_kmh`, `vehicle_count`, `visibility_m`, `temperature_c`, `humidity_pct`) range between **$-0.015$ and $+0.020$** (effectively zero).
* Meaningful statistical correlations exist exclusively between composite/logical features:
  * $\text{corr}(\text{vehicle\_count}, \text{traffic\_flow\_rate}) = \mathbf{+0.668}$
  * $\text{corr}(\text{accident\_flag}, \text{severe\_accident\_flag}) = \mathbf{+0.688}$
* **Takeaway**: Individual sensory variables act as independent orthogonal drivers. Corridors with high risk cannot be identified by a single threshold (e.g. speed alone); multi-dimensional profiling is required.

### Outlier Audit
* IQR and $3\sigma$ Z-score audits revealed **0 extreme anomalous errors**.
* Speeds (5–119 km/h) and volumes (10–499) are bounded natural observations and were 100% retained.

---

## 10. Prioritized Business Questions for Dashboard Development

Based on the empirical findings, the following business questions are ranked to establish the functional requirements for **Step 05 (KPI Definition)** and **Step 06 (Executive Dashboard)**:

| Priority | Category | Strategic Business Question | Analytical Target / Decision Value |
| :---: | :--- | :--- | :--- |
| **Critical** | **Safety Intervention** | **Which specific highway corridors (`RD-137`, `RD-170`, `RD-136`) exhibit lethal collision clusters requiring immediate physical barriers and radar enforcement?** | Eliminates fatal crash clusters by focusing interventions on the top 2% of dangerous corridors. |
| **Critical** | **Alert Re-engineering** | **How can the automated safety alert system be redesigned to eliminate its 90.3% false-alarm rate while improving its 18.6% crash capture rate?** | Reduces driver alert fatigue; provides reliable real-time collision early warnings. |
| **High** | **Temporal Operations** | **What operational countermeasures can reduce nighttime collision rates (11.64%), which represent over 41% of all network crashes?** | Guides nighttime highway patrol scheduling, reflective road stud deployment, and nighttime speed zones. |
| **High** | **Regional Resource Allocation** | **How should safety and highway maintenance budgets be prioritized between the Northern (31.2% volume) and Southern (26.3% volume) arterial networks?** | Equips regional directors with data-backed corridor utilization and safety scorecards. |
| **Medium** | **Modal Traffic Flow** | **How do commercial heavy vehicles (Trucks and Buses) compare to passenger vehicles in velocity and incident involvement across corridor types?** | Determines feasibility of freight-only lanes and heavy-vehicle speed governor audits. |
| **Medium** | **Dynamic Environmental Controls** | **What threshold combinations of poor visibility (<1,000m) and adverse road conditions (wet/slippery) justify automated variable speed limit reductions?** | Establishes automated rules for Intelligent Transportation System (ITS) variable message signs. |
| **Low** | **Macro Trend Tracking** | **How has overall network volume and safety performed year-over-year from 2018 through 2023, accounting for 2020 pandemic anomalies?** | Supplies executive leadership with annual safety benchmarks and multi-year KPI tracking. |

---

## 11. Verification Checklist

* $\checkmark$ Clean dataset loaded successfully from `data/processed/transportation_cleaned.parquet` & `.csv`.
* $\checkmark$ All 35 columns categorized into Identifiers, Dates, Dimensions, and Measures.
* $\checkmark$ Transportation volume and corridor velocity evaluated.
* $\checkmark$ Hazard warning system audited with full confusion matrix.
* $\checkmark$ Nighttime collision peak ($11.64\%$) identified and documented.
* $\checkmark$ High-risk corridor anomalies identified (`RD-137` fatality rate: $33.3\%$).
* $\checkmark$ Six high-resolution Matplotlib/Seaborn visualization figures generated in `outputs/figures/`.
* $\checkmark$ Seven prioritized business questions formulated for Step 05.

The project is ready to proceed to **Step 05 — Business Questions & KPI Definition**.
