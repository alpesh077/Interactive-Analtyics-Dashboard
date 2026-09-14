# Transportation Analytics Dashboard — Business Requirements Document (BRD)
**Project Milestone**: Step 05 — Business Questions & KPI Definition  
**Target Delivery**: Interactive Streamlit Executive Dashboard  
**Author**: Senior Transportation Business Analyst & Analytics Consultant  
**Dataset Reference**: `data/processed/transportation_cleaned.parquet` (5,500 records, 35 attributes)  
**Status**: Formally Defined & Production-Ready

---

## 1. Executive Business Objective

The primary objective of the **Transportation Analytics Dashboard** is to transform multi-dimensional highway telemetry, traffic surveillance, atmospheric sensor logs, and incident reports into real-time operational decision support for transportation executives, regional corridor managers, and safety engineers.

The interactive solution directly enables management to:
1. **Monitor Network Throughput**: Track vehicular volumes, modal splits, and velocity health across national highway corridors.
2. **Mitigate Lethal Corridor Hazards**: Identify localized collision hotspots (e.g. `RD-137` with a $44.4\%$ collision rate and $33.3\%$ fatality rate) to target physical safety barriers, radar speed enforcement, and maintenance.
3. **Re-engineer Hazard Warning Systems**: Quantify and eliminate the $90.3\%$ false-alarm rate while elevating the current $18.6\%$ collision detection sensitivity.
4. **Target High-Risk Operational Dayparts**: Address the nighttime collision peak ($11.64\%$ collision probability accounting for $41\%$ of network fatalities) via targeted lighting and patrol scheduling.
5. **Optimize Regional Infrastructure Allocation**: Allocate state and regional maintenance budgets based on concrete corridor flow and safety metrics.

---

## 2. Business Stakeholder Decision Matrix

| Stakeholder Role | Key Decisions Supported | Required Information & Metrics | Analytical Granularity |
| :--- | :--- | :--- | :--- |
| **Chief Transportation Officer / Executive Director** | • Multi-year infrastructure capital allocation<br>• National traffic safety policy & Vision Zero goals<br>• Automated alert system capital investment | • Network volume throughput<br>• Multi-year safety trends (2018–2023)<br>• Fatality and severe collision ratios<br>• National alert system recall vs false alarm rate | High-level executive KPI cards, yearly trajectories, regional macro-comparisons |
| **Regional Highway Operations Manager** | • Regional maintenance crew scheduling<br>• Lane expansion prioritization<br>• Regional congestion bottleneck intervention | • Regional volume splits (North, South, East, West, Central)<br>• Traffic density distribution<br>• Average corridor velocity<br>• Flow momentum by region | Regional scorecards, cross-regional comparative bar charts |
| **Corridor Safety & Traffic Engineer** | • Installation of rumble strips, crash barriers & signage<br>• Corridor-specific speed limit adjustments<br>• Blackspot geometry redesign | • High-risk corridor ranking (`road_id`)<br>• Collision severity breakdown (Fatal, Major, Minor)<br>• Pavement condition vs accident rate<br>• Corridors with $\ge 20\%$ accident rate | Segment-level detail, corridor ranking tables, scatterplots |
| **Automated Hazard & Dispatch Systems Lead** | • Warning algorithm sensitivity & threshold tuning<br>• Sensor telemetry calibration<br>• Mitigation of driver and operator alert fatigue | • Alert recall (sensitivity) & false alarm rate<br>• Dispatched alert volumes vs confirmed crashes<br>• Multi-factor alert trigger rules | Confusion matrix charts, operational trigger logs |
| **Commercial Fleet & Freight Operations Lead** | • Fleet routing optimization<br>• Heavy vehicle dispatch scheduling<br>• Nighttime driver safety protocols | • Modal breakdown (Trucks, Buses, Cars, Bikes)<br>• Modal travel velocity & crash involvement<br>• Daypart safety risk premiums | Modal performance comparisons, daypart risk charts |

---

## 3. Prioritized Business Questions Catalog

Discovered during Step 04 EDA, each question is formally evaluated using a 1–5 scoring rubric:
* **Business Impact (1–5)**: Direct effect on lives saved, congestion reduction, and operational cost.
* **Data Availability (1–5)**: Completeness and reliability of source fields in the cleaned dataset.
* **Actionability (1–5)**: Ability of management to enact a policy or operational change.
* **Decision Value (1–5)**: Strategic significance of the decision supported.

$$\text{Composite Priority Score} = \text{Impact} + \text{Data Availability} + \text{Actionability} + \text{Decision Value} \quad (\text{Max } 20)$$

| ID | Strategic Business Question | Category | Impact | Data Avail | Action | Dec Value | Total Score | Priority Tier |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **BQ-01** | **Which specific highway corridors (`RD-137`, `RD-170`, `RD-136`) exhibit lethal collision clusters requiring immediate physical barriers and radar enforcement?** | Safety Intervention | 5 | 5 | 5 | 5 | **20 / 20** | **Critical** |
| **BQ-02** | **How can the automated safety alert system be redesigned to eliminate its 90.3% false-alarm rate while improving its 18.6% crash capture rate?** | Alert Optimization | 5 | 5 | 5 | 5 | **20 / 20** | **Critical** |
| **BQ-03** | **What operational countermeasures can reduce nighttime collision rates (11.64%), which represent over 41% of all network crashes?** | Daypart Safety | 5 | 5 | 4 | 5 | **19 / 20** | **Critical** |
| **BQ-04** | **How should safety and highway maintenance budgets be prioritized between the Northern (31.2% volume) and Southern (26.3% volume) arterial networks?** | Regional Allocation | 4 | 5 | 5 | 4 | **18 / 20** | **High** |
| **BQ-05** | **How do commercial heavy vehicles (Trucks and Buses) compare to passenger vehicles in velocity and incident involvement across corridor types?** | Modal Fleet Flow | 4 | 5 | 4 | 4 | **17 / 20** | **High** |
| **BQ-06** | **What threshold combinations of poor visibility (<1,000m) and adverse road conditions (wet/slippery) justify automated variable speed limit reductions?** | Environmental Risk | 4 | 5 | 4 | 4 | **17 / 20** | **High** |
| **BQ-07** | **How has overall network volume and safety performed year-over-year from 2018 through 2023, accounting for 2020 pandemic anomalies?** | Macro Trends | 3 | 5 | 3 | 4 | **15 / 20** | **Medium** |
| **BQ-08** | **Which road corridors maintain optimal free-flow speeds (55–65 km/h) even under High and Very High traffic density conditions?** | Traffic Flow | 3 | 5 | 4 | 3 | **15 / 20** | **Medium** |

---

## 4. Key Performance Indicator (KPI) Framework

The transportation KPI framework is organized into 5 operational domains:

```text
Transportation Analytics KPI Architecture
│
├── 1. Throughput & Velocity KPIs
│   ├── Total Monitored Volume (Vehicles)
│   ├── Average Network Velocity (km/h)
│   └── Traffic Flow Momentum Index
│
├── 2. Corridor Safety & Risk KPIs
│   ├── Total Confirmed Collisions (Count)
│   ├── Network Collision Rate (%)
│   ├── Lethal Fatality Ratio (%)
│   ├── Severe Crash Index (%)
│   └── High-Risk Corridor Share (%)
│
├── 3. Hazard Warning System Reliability KPIs
│   ├── Warning Alert Recall / Sensitivity (%)
│   ├── Alert False Alarm Rate (%)
│   └── Warning Dispatch Volume (Alerts)
│
├── 4. Operational & Daypart Risk KPIs
│   ├── Nighttime Risk Premium (% excess crash rate)
│   └── Peak-Hour Flow Efficiency
│
└── 5. Environmental & Pavement Resilience KPIs
    └── Adverse Weather Risk Multiplier
```

---

## 5. Formal KPI Definition Table

| KPI Name | Domain | Exact Mathematical Formula | Source Data Attributes | Business Purpose | Baseline (Dataset) | Target / Threshold |
| :--- | :--- | :--- | :--- | :--- | :---: | :--- |
| **Total Monitored Volume** | Throughput | $\sum \text{vehicle\_count}$ | `vehicle_count` | Cumulative traffic volume observed | 1,397,508 | Requires Business Validation |
| **Average Network Velocity** | Velocity | $\frac{1}{N}\sum \text{avg\_speed\_kmh}$ | `avg_speed_kmh` | Benchmark travel speed across corridors | 61.3 km/h | 55.0 – 65.0 km/h (Optimal) |
| **Traffic Flow Momentum** | Flow | $\frac{1}{N}\sum (\text{vehicle\_count} \times \text{avg\_speed\_kmh})$ | `traffic_flow_rate` | Volume-velocity flow intensity index | 15,558.4 | Requires Business Validation |
| **Total Collisions** | Safety | $\sum \text{accident\_flag}$ | `accident_flag` | Total confirmed collision events | 575 crashes | Zero Vision ($<5\%$ incidence) |
| **Network Collision Rate** | Safety | $\frac{\sum \text{accident\_flag}}{N} \times 100$ | `accident_flag`, `record_id` | Overall probability of a crash per observation | 10.45% | $< 8.00\%$ |
| **Lethal Fatality Ratio** | Safety | $\frac{\sum \text{fatal\_accident\_flag}}{\sum \text{accident\_flag}} \times 100$ | `fatal_accident_flag`, `accident_flag` | Proportion of crashes causing fatalities | 25.57% | $< 15.00\%$ |
| **Severe Crash Index** | Safety | $\frac{\sum \text{severe\_accident\_flag}}{\sum \text{accident\_flag}} \times 100$ | `severe_accident_flag`, `accident_flag` | Proportion of fatal and major injury crashes | 50.09% | $< 35.00\%$ |
| **High-Risk Corridors Share** | Safety | $\frac{\text{Count}(\text{Corridor Rate} \ge 20\%)}{\text{Total Corridors}} \times 100$ | `road_id`, `accident_flag` | Concentration of acute hazard corridors | 1.4% (7 corridors) | 0.0% (Zero Blackspots) |
| **Warning Alert Recall** | Reliability | $\frac{\text{True Positives}}{\text{Total Accidents}} \times 100$ | `alert_flag`, `accident_flag` | Sensitivity: % crashes preceded by warning | 18.61% | $> 75.00\%$ |
| **Alert False Alarm Rate** | Reliability | $\frac{\text{False Positives}}{\text{Total Alerts Dispatched}} \times 100$ | `alert_flag`, `accident_flag` | Precision inverse: % unconfirmed alerts | 90.30% | $< 25.00\%$ |
| **Nighttime Risk Premium** | Daypart | $\text{Rate}_{\text{Night}} - \text{Rate}_{\text{Day}}$ | `time_of_day`, `accident_flag` | Excess crash probability during night hours | +1.89% (+19.3% relative) | $\le 0.00\%$ (Parity) |
| **Weather Risk Multiplier** | Atmosphere | $\frac{\text{Rate}_{\text{Adverse Weather}}}{\text{Rate}_{\text{Clear}}}$ | `weather`, `accident_flag` | Relative crash risk elevation under adverse weather | 0.98x | Requires Business Validation |

---

## 6. Dashboard Sections & Decision Areas

The future executive dashboard will be structured into 6 dedicated operational decision areas:

### Section 1: Executive Overview & Network Pulse
* **Business Question**: How is the national transportation corridor network performing right now across volume, velocity, and incident safety?
* **Core KPIs**: Total Volume, Mean Speed, Total Collisions, Collision Rate, Fatality Ratio, Alert Recall.
* **Key Visuals**: KPI Executive Summary Cards, Multi-Year Volume & Incident Trend Line, Regional Throughput Donut/Bar.
* **Decision Supported**: C-level situational awareness; macro trend evaluation.

### Section 2: Corridor Risk & Lethal Hotspots
* **Business Question**: Which specific highway segments require urgent engineering barriers, radar speed traps, or pavement maintenance?
* **Core KPIs**: Corridor Accident Rate (%), Fatality Rate (%), Severe Crash Index (%), High-Risk Corridors Count.
* **Key Visuals**: Top 10 High-Risk Corridors Ranked Bar Chart, Corridor Risk vs Volume Scatterplot, Pavement Condition Impact Bar.
* **Decision Supported**: Prioritizes safety capital investments on deadly corridors (`RD-137`, `RD-170`, `RD-136`).

### Section 3: Traffic Flow & Congestion Dynamics
* **Business Question**: How does congestion density impact vehicle speed across dayparts, and where are the network bottlenecks?
* **Core KPIs**: Traffic Density Score (1–4), Average Speed by Density, Traffic Flow Momentum.
* **Key Visuals**: Velocity Boxplot across Traffic Density Tiers, Daypart Volume vs Speed Dual-Axis Chart.
* **Decision Supported**: Intelligent traffic signal timing, congestion tolling, and lane re-allocations.

### Section 4: Hazard Alert System Performance & Optimization
* **Business Question**: How reliable is the automated hazard warning system, and how much false-alarm overhead is it imposing on drivers?
* **Core KPIs**: Dispatched Warnings (1,103), Alert Recall (18.6%), False Alarm Rate (90.3%), Missed Incidents (468).
* **Key Visuals**: Alert Classification Confusion Matrix Bar, Alert Efficiency Waterfall, Daypart Alert Sensitivity.
* **Decision Supported**: Algorithm threshold tuning; reduction of driver alert fatigue.

### Section 5: Regional & Modal Slicing
* **Business Question**: How do transportation patterns differ between the 5 geographic regions and 5 vehicle modal classes?
* **Core KPIs**: Regional Volume Share (%), Modal Average Speed, Regional Collision Rate.
* **Key Visuals**: 5-Region Comparative Scorecard, Modal Speed vs Collision Rate Grouped Bar.
* **Decision Supported**: State-level transport funding; freight corridor lane assignments.

### Section 6: Telemetry Data Explorer & Raw Records Audit
* **Business Question**: Can operational managers inspect raw telemetry logs, filter specific road segments, and export audit data?
* **Key Features**: Interactive multi-filter data table, column selector, CSV/Parquet export button.
* **Decision Supported**: Root-cause incident post-mortems and compliance reporting.

---

## 7. Dimensional Hierarchy & Filter Architecture

To provide intuitive interactivity without cognitive overload, filters are partitioned into two tiers:

```text
Interactive Filter Hierarchy
│
├── Primary Global Filters (Sidebar)
│   ├── Date Range / Year Selector (2018 - 2023)
│   ├── Geographic Region Multi-Select (North, South, East, West, Central)
│   ├── Vehicle Class Filter (Bikes, Buses, Cars, Trucks, Mixed)
│   └── Traffic Density Filter (Low, Medium, High, Very High)
│
└── Secondary Contextual Filters (In-Page Expander)
    ├── Operational Daypart (Morning Peak, Midday, Evening Peak, Night)
    ├── Prevailing Weather (Clear, Fog, Rain, Snow, Storm)
    ├── Road Surface Condition (Dry, Wet, Slippery, Under Maintenance)
    └── Accident Severity Filter (Fatal, Major, Minor, Unspecified)
```

---

## 8. Business Question $\rightarrow$ KPI $\rightarrow$ Visualization Mapping Matrix

| Strategic Business Question | Primary KPI | Analytical Dimension | Recommended Visual | Executive Decision Supported |
| :--- | :--- | :--- | :--- | :--- |
| **BQ-01: Which corridors have lethal crash clusters?** | Corridor Accident Rate & Fatality Count | `road_id` | Horizontal Ranked Bar Chart & Scatterplot | Immediate highway barrier installation & patrol placement on `RD-137` |
| **BQ-02: Why is the alert false alarm rate 90.3%?** | Alert Recall & False Alarm Rate | `alert_success_type` | 4-Way Confusion Classification Bar | Machine learning alert model re-training to reduce driver fatigue |
| **BQ-03: Why do night hours experience 41% of crashes?** | Nighttime Risk Premium & Night Crash Rate | `time_of_day`, `hour` | Dual-Axis Volume vs Accident Rate Chart | Highway lighting upgrades & night commercial patrol shifts |
| **BQ-04: How should regional maintenance funds be split?** | Regional Volume Share & Collision Rate | `region` | Grouped Bar Chart & Regional Scorecard | Budget allocation favoring North (31.2%) and South (26.3%) corridors |
| **BQ-05: How do heavy trucks compare to cars in speed/risk?** | Modal Velocity & Collision Rate | `vehicle_type` | Comparative Bar Chart | Dedicated heavy freight lanes and speed governor compliance |
| **BQ-06: Do low visibility & wet roads compound crash risk?** | Weather & Pavement Collision Rate | `weather`, `road_condition` | Cross-Tabulated Heatmap | Automated variable speed limit reductions during rain/fog |
| **BQ-07: What is the 6-year macro trend of traffic & crashes?** | Total Volume & Collision Trajectory | `year`, `quarter` | Multi-Year Line Trend with 2020 Annotation | Executive annual performance reviews and safety benchmarks |

---

## 9. Operational Alert Opportunities & Trigger Thresholds

The future dashboard will incorporate visual status indicators and callout alerts when metrics breach operational bounds:

| Operational Condition | Detection Metric | Trigger Threshold | Alert Severity | Operational Action Required |
| :--- | :--- | :---: | :---: | :--- |
| **Critical Corridor Blackspot** | Corridor Collision Rate (`road_id`) | $\ge 25.0\%$ | **CRITICAL (Red)** | Dispatch highway safety engineering inspection team |
| **Lethal Cluster Alert** | Fatal Collisions on Corridor | $\ge 2$ Fatalities | **CRITICAL (Red)** | Deploy temporary radar speed signs & rumble strips |
| **Severe Alert System Fatigue** | False Alarm Rate | $> 85.0\%$ | **HIGH (Amber)** | Recalibrate sensor sensitivity; disable low-confidence warnings |
| **Nighttime Incident Surge** | Nighttime Risk Premium | $> +3.00\%$ | **MEDIUM (Yellow)** | Activate overhead highway illumination & warning beacons |
| **Adverse Weather Speed Hazard** | Average Speed in Poor Visibility ($<1\text{km}$) | $> 65\text{ km/h}$ | **HIGH (Amber)** | Display dynamic speed reduction on electronic roadside signs |

---

## 10. Data Limitations & Items Requiring Business Validation

### A. Data Limitations
1. **Absence of Retail Financial Data**: The dataset contains physical sensor and incident logs. It does not record transaction dollar revenue, unit sales price, or maintenance repair costs.
2. **Absence of Direct Fuel Telemetry**: Individual fuel consumption ($L/100km$) is not recorded; traffic flow momentum ($\text{vehicle\_count} \times \text{speed}$) serves as the operational proxy.
3. **Synthetic Independence**: Features in this benchmark dataset were generated with independent stochastic distributions, resulting in near-zero correlations between independent environmental variables.

### B. Items Requiring Formal Business Validation
1. **Monetary Dollar Cost per Collision**: A cost model (e.g. estimating Fatal crash cost at \$500k, Major crash at \$100k) should be validated with executive finance.
2. **Acceptable Alert False Alarm Ceiling**: Operational tolerance for false alarms (recommended $\le 25\%$) requires sign-off from traffic management.
3. **Corridor High-Risk Cutoff**: The $20\%$ accident rate threshold requires ratification by the Ministry of Transport or highway authority.

The business requirements and KPI framework are approved to establish the **Dashboard Blueprint**.
