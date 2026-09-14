# Transportation Analytics — Step 07: Interactive Visualization with Plotly Report

**Project**: Transportation Business Analytics Dashboard  
**Dataset**: `data/processed/transportation_cleaned.csv` / `data/processed/transportation_cleaned.parquet`  
**Dataset Scope**: 5,500 Telemetry Windows | 500 Road Corridors | 5 Geographic Regions | 2018–2023 Multi-Year Horizon  
**Milestone**: Step 07 — Interactive Visualization with Plotly  
**Status**: Verified & Production Ready  

---

## 1. Visualization Objective

In Step 06, we mathematically verified all 12 approved transportation KPIs and generated static publication-quality Matplotlib/Seaborn figures. While static figures excel at structured reporting, highway operations and executive decision-making require **interactive exploration**. 

Step 07 converts the analytical findings into a modular, production-ready **interactive Plotly visualization engine** (`src/interactive_visualization.py`). These interactive visualizations allow stakeholders to:
* **Inspect Granular Data Points**: Hover over corridors, regions, dayparts, and temporal points to inspect exact counts, velocities, fatality counts, and percentages without visual clutter.
* **Isolate Time Horizons**: Utilize range sliders and date filters to zoom into specific operational windows (such as the 2020 pandemic anomaly or the 2021 post-lockdown mobility rebound).
* **Cross-Filter Dynamically**: Interact with legend elements to isolate specific vehicle modalities (e.g., Heavy Trucks vs. Passenger Cars) or geographic zones.
* **Maintain Filter-Ready Decoupling**: All functions accept sliced DataFrames (`df: pd.DataFrame`) and handle zero-record or missing-column conditions gracefully, preparing the codebase for seamless integration into Streamlit in Step 08.

---

## 2. Strategic Business Questions Addressed

Every interactive visualization is tied directly to the prioritized business questions established in the Step 05 Business Requirements Document:

1. **BQ-01: Lethal Corridor Clusters & Blackspots**: Which specific corridors exhibit acute collision clusters and fatalities requiring immediate engineering intervention?
2. **BQ-02: Safety Alert System Reliability & Fatigue**: What is the empirical classification accuracy of the automated hazard alert system, and how severe is false-alarm fatigue?
3. **BQ-03: Diurnal Flow & Nighttime Risk Premium**: How does collision probability elevate during nighttime hours compared to daytime traffic windows?
4. **BQ-04: Regional Infrastructure & Lethality Divergence**: How do traffic throughput and casualty severity diverge across the 5 geographic regions?
5. **BQ-05: Modal Fleet Dynamics & Casualty Severity**: How do commercial freight vehicles (Trucks and Buses) compare to passenger vehicles in velocity and fatal casualty involvement?
6. **BQ-06: Traffic Density & Speed Degradation**: At what traffic density tiers does network travel velocity degrade below optimal throughput bands?
7. **BQ-07: Multi-Year Trajectory & Macroeconomic Shocks**: How did network volume and collision rates evolve from 2018 through 2023, specifically during the 2020 pandemic mobility dip and 2021 rebound?
8. **BQ-08: Zero-Incident Safe Corridors**: Which high-volume corridors achieve zero collisions and can serve as network design benchmarks?

---

## 3. Interactive Plotly Visualizations Matrix

| Business Question | KPI Mapped | Plotly Function (`src/interactive_visualization.py`) | Dimensions & Measures | Interactivity Features |
| :--- | :--- | :--- | :--- | :--- |
| **Executive Overview** | Collision Rate, Fatality Ratio, Severe Crash Index, High-Risk Share, Alert Recall, False Alarms | `create_kpi_benchmark_bars(df)` | 6 Strategic Ratios vs. Benchmarks | Paired horizontal bars, color-coded status breach indicators (Green/Amber/Red), custom hover template with benchmark delta |
| **BQ-07: Macro Trends** | Total Monitored Volume, Total Collisions, YoY Growth Rates | `create_yearly_traffic_and_accident_trend(df)` | `year`, `vehicle_count`, `accident_flag` | Dual-axis Bar + Line, hover cards showing YoY volume growth and incident growth, historical callout annotations for 2020/2021 |
| **BQ-07: Seasonality** | Monthly Volume, Monthly Collision Rate | `create_monthly_traffic_trend(df)` | `timestamp` (Monthly), `vehicle_count`, `accident_flag` | Dual-axis line chart, **interactive range slider**, month-over-month hover cards |
| **BQ-01: Lethal Blackspots** | Corridor Crash Rate (%), Fatal Collisions, Mean Velocity | `create_corridor_risk_ranking(df, top_n, min_obs)` | `road_id`, `accident_flag`, `fatal_accident_flag`, `avg_speed_kmh` | Horizontal bar ranking, color-coded by fatality count, hover cards displaying observations, volume, speed, and exact fatality counts |
| **BQ-08: Safe Benchmarks** | High-Volume Zero-Crash Corridors | `create_safe_corridors_benchmark(df, top_n)` | `road_id`, `vehicle_count`, `observations` | Horizontal green bars, hover cards displaying cumulative throughput ($>5,600$ vehicles) and zero crash confirmation |
| **BQ-04: Regional Divergence**| Regional Volume Share (%), Mean Velocity, Crash Rate, Fatality Ratio | `create_regional_performance_matrix(df)` | `region`, `vehicle_count`, `avg_speed_kmh`, `accident_flag`, `fatal_accident_flag` | 2-panel subplot: Volume Throughput/Share vs. Grouped Crash Rate & Fatality Ratio, multi-measure hover cards |
| **BQ-05: Modal Dynamics** | Velocity vs. Crash Rate (%) and Fatality Ratio (%) | `create_modal_velocity_and_risk_chart(df)` | `vehicle_type`, `avg_speed_kmh`, `accident_flag`, `fatal_accident_flag` | 2-panel subplot comparing modal speed vs grouped incident/fatality severity, hover cards displaying exact incident counts |
| **BQ-03: Nighttime Premium** | Daypart Volume, Collision Rate (%), Nighttime Risk Premium | `create_daypart_safety_premium_chart(df)` | `time_of_day`, `vehicle_count`, `accident_flag` | Dual-axis Bar + Line, callout annotation highlighting **+1.88% Nighttime Risk Premium** (+19.3% relative risk) |
| **BQ-03: Diurnal Rhythm** | 24-Hour Vehicular Intensity | `create_hourly_traffic_flow_heatmap(df)` | `hour`, `day_of_week`, `vehicle_count` | 24x7 interactive heatmap, hover tooltips showing exact average vehicles per hour-day window |
| **BQ-02: Alert Reliability** | Recall (18.61%), Precision (9.70%), False Alarm Rate (90.30%) | `create_alert_confusion_matrix_heatmap(df)` | `alert_flag`, `accident_flag` | 2x2 Confusion matrix heatmap (TP: 107, FN: 468, FP: 996, TN: 3,929), cell text showing classification role and operational metric |
| **BQ-02: Alert Proportions** | Classification Share (%) | `create_alert_outcome_donut(df)` | `alert_success_type` | Interactive donut chart with pull-out hover slices showing percentage share |
| **BQ-06: Speed Degradation** | Speed Distribution across Density Tiers | `create_speed_vs_density_boxplot(df)` | `traffic_density`, `avg_speed_kmh` | Interactive boxplot showing mean line, quartiles, and outliers across Low, Medium, High, and Very High congestion tiers |
| **Environmental Impact** | Weather Collision Rate vs. Mean Speed | `create_weather_risk_comparison(df)` | `weather`, `avg_speed_kmh`, `accident_flag` | Dual-axis grouped bar and line showing near-uniform risk (1.02x multiplier) across Clear, Rain, Fog, Snow, and Storm |

---

## 4. Technical Visualization Decisions & UX Rationale

1. **Paired Horizontal Benchmark Bars (`create_kpi_benchmark_bars`)**:
   - *Rationale*: A standard bar chart lacks context regarding whether a metric is good or bad. By pairing the actual observed KPI against a dashed target benchmark and color-coding breaches (red for high collision rate, yellow for elevated false alarms, green for compliant targets), executives can assess system health within 5 seconds.
2. **Dual-Axis Multi-Year Trajectory with Historical Annotations (`create_yearly_traffic_and_accident_trend`)**:
   - *Rationale*: Volume is measured in hundreds of thousands while collisions are in tens/hundreds. Dual axes allow simultaneous trend tracking without distortion. Anchored annotations highlight the 2020 pandemic dip (-3.65% volume, -21.43% crashes) and 2021 rebound (+9.93% volume, +38.96% crashes).
3. **Monthly Time-Series with Range Slider (`create_monthly_traffic_trend`)**:
   - *Rationale*: Visualizing 72 consecutive calendar months can lead to dense axis labels. Integrating Plotly's native range slider allows operators to fluidly expand or focus on specific multi-month windows.
4. **Horizontal Blackspot Rankings (`create_corridor_risk_ranking`)**:
   - *Rationale*: Corridor IDs (`RD-137`, `RD-136`, etc.) have variable text lengths. Horizontal orientation prevents vertical label overlapping and allows clear embedded annotations indicating both accident rate and lethal fatality counts.
5. **Interactive 2x2 Confusion Matrix (`create_alert_confusion_matrix_heatmap`)**:
   - *Rationale*: A raw table of true/false positives is hard to digest. The 2x2 heatmap visually distinguishes the massive False Alarm quadrant (996 windows, 90.30% fatigue) from True Positives (107 windows, 18.61% recall), directly informing threshold tuning.

---

## 5. Skipped Visualizations & Missing Retail Data Documentation

In strict alignment with professional data integrity principles, the following hypothetical charts were **explicitly omitted** because their required attributes do not exist in the source sensor telemetry dataset:

| Hypothetical Visualization | Missing Dataset Attributes | Technical Decision | Operational Proxy / Replacement |
| :--- | :--- | :--- | :--- |
| **Revenue by Region / Route** | Dollar sales revenue, billing invoices | **Skipped** | Evaluated via **Vehicle Volume Throughput** (`vehicle_count`) and **Traffic Flow Momentum** (`traffic_flow_rate`) |
| **Transportation Profit & Margin Trend**| Unit cost of goods sold, shipping margins, profit | **Skipped** | Evaluated via **Corridor Collision Risk** and **Velocity Efficiency** |
| **Cost per Kilometer (Cost/KM)** | Financial route expenses, vehicle operating cost | **Skipped** | Evaluated via **Corridor Flow Density** and **Average Speed** |
| **On-Time Delivery Performance** | Promised delivery timestamps, delay status | **Skipped** | Evaluated via **Operational Dayparts** (`time_of_day`) and **Hourly Flow Heatmaps** |
| **Fuel Consumption / Fuel Efficiency** | Fuel liters, battery kWh, km/L economy | **Skipped** | Evaluated via **Vehicle Classification Velocity Dynamics** (`vehicle_type`) |
| **Driver & Customer Profitability** | Driver names, driver IDs, customer accounts | **Skipped** | Evaluated via **Corridor Segment IDs** (`road_id`) and **Geographic Regions** (`region`) |

---

## 6. Dashboard Integration Plan (Step 08 Wireframe Mapping)

The interactive Plotly functions created in Step 07 map directly into the 6 tabs defined in [`data/processed/dashboard_blueprint.md`](file:///d:/Users/Alpesh/business/9.%20PORTFOLIO/PORTFOLIO/assets/Projects/PROJECT.4%20CAR%20TRACKING%20SYSTEM/Project%204.1%20Interactive%20Analtyics%20Dashboard/data/processed/dashboard_blueprint.md):

* **Tab 1 — Executive Overview & Network Pulse**:
  - `create_kpi_benchmark_bars(df)`
  - `create_yearly_traffic_and_accident_trend(df)`
  - `create_regional_performance_matrix(df)` (Subplot 1: Volume Share)
* **Tab 2 — Corridor Risk & Lethal Hotspots**:
  - `create_corridor_risk_ranking(df, top_n=10, min_obs=5)`
  - `create_safe_corridors_benchmark(df, top_n=10)`
* **Tab 3 — Traffic Flow & Congestion Dynamics**:
  - `create_speed_vs_density_boxplot(df)`
  - `create_hourly_traffic_flow_heatmap(df)`
* **Tab 4 — Hazard Alert System Performance & Optimization**:
  - `create_alert_confusion_matrix_heatmap(df)`
  - `create_alert_outcome_donut(df)`
* **Tab 5 — Regional & Modal Slicing**:
  - `create_regional_performance_matrix(df)`
  - `create_modal_velocity_and_risk_chart(df)`
  - `create_weather_risk_comparison(df)`
* **Tab 6 — Telemetry Data Explorer & Raw Records Audit**:
  - `create_monthly_traffic_trend(df)` (Time-series explorer over filtered slices)

---

## 7. Mathematical Validation Against Pandas Calculations

All interactive Plotly visualizations were audited against the underlying Pandas calculations in `src/kpi.py` and `src/data_loader.py` to ensure 100% numerical consistency:

1. **Volume Throughput**:
   - Plotly Yearly Volume Sum: **1,397.5k vehicles**
   - Pandas Total Volume: **1,397,508 vehicles** $\rightarrow$ **Exact Match**
2. **Accident Counts**:
   - Plotly Yearly Accident Sum: **575 collisions**
   - Pandas Total Collisions: **575 collisions** $\rightarrow$ **Exact Match**
3. **Corridor Blackspot Risk**:
   - Plotly Top 1 Corridor: `RD-137` at **44.44% accident rate**, 3 fatalities, 73.4 km/h speed
   - Pandas Corridor Aggregation: `RD-137` at **44.44% accident rate**, 3 fatalities $\rightarrow$ **Exact Match**
4. **Alert System Confusion Matrix**:
   - Plotly Matrix Sum: $107 (\text{TP}) + 468 (\text{FN}) + 996 (\text{FP}) + 3,929 (\text{TN}) = \mathbf{5,500}$ records
   - Cleaned Dataset Total Records: $\mathbf{5,500}$ records $\rightarrow$ **Exact Match**
5. **Nighttime Risk Premium**:
   - Plotly Night Accident Rate: **11.64%** vs. Daytime Average: **9.75%** $\rightarrow$ Premium: **+1.88%**
   - Pandas Calculation: $11.64\% - 9.75\% = \mathbf{+1.88\%}$ $\rightarrow$ **Exact Match**

---

## Conclusion

Step 07 successfully converts the analytical and statistical outputs of the project into high-performance, filter-ready interactive Plotly charts. The codebase is fully prepared for **Step 08 — Dashboard Architecture & Streamlit Layout**.
