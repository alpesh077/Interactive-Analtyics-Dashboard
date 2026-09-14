# Transportation Analytics — Step 06: KPI Calculation & Analytical Visualization Report

**Project**: Transportation Business Analytics Dashboard  
**Dataset**: `data/processed/transportation_cleaned.csv` / `data/processed/transportation_cleaned.parquet`  
**Dataset Scope**: 5,500 Telemetry Windows | 500 Road Corridors | 5 Geographic Regions | 2018–2023 Multi-Year Horizon  
**Milestone**: Step 06 — KPI Calculation, Dimension Breakdown, Growth Modeling & Analytical Visualization  
**Status**: Verified & Production Ready  

---

## 1. Executive Summary & Production KPI Framework

Step 06 transforms the business framework and dashboard blueprint established in Step 05 into fully functional, production-tested Python KPI calculations and publication-quality analytical visualizations. 

All 12 approved transportation metrics have been implemented inside `src/kpi.py` using robust numerical protections against division-by-zero, empty slices, and null attributes. Empirical values reflect the verified, deduplicated cleaned dataset of 5,500 observation windows and 1,397,508 monitored vehicles.

### Production KPI Summary Table

| KPI Name | Empirical Value | Unit | Category | Calculation Status | Strategic Objective / Target Benchmark |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Total Monitored Volume** | **1,397,508** | Vehicles | Throughput & Flow | Valid | Baseline network throughput across 500 corridors |
| **Average Network Velocity** | **61.3** | km/h | Throughput & Flow | Valid | Maintain safe optimal speed band (55.0 – 65.0 km/h) |
| **Traffic Flow Momentum** | **15,558.4** | Flow Index | Throughput & Flow | Valid | Composite index ($Volume \times Speed$) tracking mobility mass |
| **Total Collisions** | **575** | Incidents | Corridor Safety | Valid | Absolute incidence count over 6-year operational history |
| **Network Collision Rate** | **10.45%** | % | Corridor Safety | Valid | Target: $< 8.00\%$ overall collision incidence rate |
| **Lethal Fatality Ratio** | **25.57%** | % | Corridor Safety | Valid | Vision Zero Target: $< 15.00\%$ fatal crash share (147 deaths) |
| **Severe Crash Index** | **50.09%** | % | Corridor Safety | Valid | Target: $< 35.00\%$ (288 combined Fatal & Major crashes) |
| **High-Risk Corridors Share** | **18.33%** | % | Corridor Safety | Valid | Target: $< 5.00\%$ (90 of 491 qualified corridors $\ge 20\%$ crash rate) |
| **Hazard Alert Recall (Sensitivity)** | **18.61%** | % | Warning Reliability | Valid | Safety-Critical Target: $> 75.00\%$ (Only 107 of 575 crashes preceded) |
| **Alert False Alarm Rate** | **90.30%** | % | Warning Reliability | Valid | Target: $< 25.00\%$ alarm fatigue threshold (996 of 1,103 alerts false) |
| **Nighttime Risk Premium** | **+1.88%** | % | Temporal Operations | Valid | Target: $\le 0.00\%$ (Night crash rate 11.64% vs Day 9.75%; +19.3% relative risk) |
| **Adverse Weather Risk Multiplier** | **1.02x** | Ratio | Atmospheric Impact | Valid | Near uniform risk across Clear (10.25%) vs Adverse (10.50%) |

---

## 2. Multi-Dimensional Performance Analysis

### A. Geographic Regional Performance

The transportation network spans 5 designated geographic zones across India. Demand and risk are highly concentrated in the Northern and Southern corridors, which together account for **57.5%** of all vehicle volume and **62.6%** of all collision incidents.

| Region | Monitored Records | Total Volume | Volume Share (%) | Mean Velocity (km/h) | Flow Momentum | Total Crashes | Crash Rate (%) | Fatalities | Fatality Ratio (%) | Severe Crashes | Total Alerts | Alert Rate (%) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **North** | 1,725 | 436,191 | 31.21% | 60.9 | 15,406.3 | 196 | **11.36%** | 45 | 22.96% | 104 | 336 | 19.48% |
| **South** | 1,480 | 367,747 | 26.31% | 61.3 | 15,103.9 | 164 | **11.08%** | 37 | 22.56% | 71 | 312 | 21.08% |
| **East** | 927 | 241,835 | 17.30% | 63.2 | 16,323.7 | 87 | **9.39%** | 23 | 26.44% | 45 | 192 | 20.71% |
| **Central** | 805 | 208,215 | 14.90% | 61.7 | 16,067.0 | 73 | **9.07%** | 25 | **34.25%** | 41 | 151 | 18.76% |
| **West** | 563 | 143,520 | 10.27% | 59.4 | 15,231.2 | 55 | **9.77%** | 17 | 30.91% | 27 | 112 | 19.89% |

**Key Dimension Takeaways**:
1. **Volume & Accident Hotspots**: The North region records the highest volume (436,191) and the highest accident rate (11.36%), followed closely by the South (11.08%).
2. **Lethality Severity Anomaly**: While Central and West experience lower absolute volume and crash rates ($<10\%$), their **fatality ratios are significantly worse** (Central: 34.25%; West: 30.91%). When collisions occur in Central and West, they are far more likely to be fatal, suggesting higher intercity speeds, delayed emergency response times, or sparse trauma care access.

---

### B. Fleet Vehicle Modality Performance

Traffic records represent 5 operational vehicle classifications. Throughput is evenly distributed across modal classes, but safety and fatality profiles diverge significantly.

| Vehicle Type | Observations | Total Volume | Volume Share (%) | Mean Velocity (km/h) | Total Crashes | Crash Rate (%) | Fatalities | Fatality Ratio (%) | Severe Crashes | Severe Crash Share (%) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Car** | 1,172 | 299,650 | 21.44% | 60.2 | 128 | 10.92% | 40 | **31.25%** | 68 | 53.12% |
| **Bus** | 1,103 | 279,387 | 19.99% | 60.5 | 119 | 10.79% | 26 | 21.85% | 61 | 51.26% |
| **Mixed** | 1,096 | 279,868 | 20.03% | 61.2 | 113 | 10.31% | 24 | 21.24% | 57 | 50.44% |
| **Truck** | 1,058 | 272,079 | 19.47% | 61.9 | 102 | 9.64% | 32 | **31.37%** | 52 | 50.98% |
| **Bike** | 1,071 | 266,524 | 19.07% | 63.0 | 113 | 10.55% | 25 | 22.12% | 50 | 44.25% |

**Key Modal Takeaways**:
1. **Lethality Concentration in Freight and Passenger Cars**: Cars (31.25%) and Heavy Commercial Trucks (31.37%) display markedly elevated fatality ratios compared to Buses (21.85%) and Mixed traffic (21.24%).
2. **Speed Distribution**: Two-wheelers/Bikes maintain the highest average velocity (63.0 km/h), but exhibit lower severe crash share (44.25%) than passenger cars (53.12%).

---

### C. Operational Daypart Performance

| Daypart Window | Hours Included | Observations | Volume (k) | Volume Share | Mean Speed | Total Crashes | Crash Rate (%) | Fatalities | Fatality Ratio (%) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Night** | 22:00 – 06:59 | 2,028 | 516.5k | 36.96% | 62.0 km/h | 236 | **11.64%** | 65 | 27.54% |
| **Evening Peak**| 17:00 – 21:59 | 1,200 | 302.6k | 21.66% | 60.7 km/h | 118 | **9.83%** | 27 | 22.88% |
| **Midday** | 10:00 – 16:59 | 1,370 | 347.5k | 24.87% | 59.5 km/h | 128 | **9.34%** | 33 | 25.78% |
| **Morning Peak**| 07:00 – 09:59 | 902 | 230.9k | 16.52% | 63.5 km/h | 93 | **10.31%** | 22 | 23.66% |

**Daypart Insights**:
- **Nighttime Risk Premium**: The collision rate during nighttime hours is **11.64%**, compared to **9.75%** across all daytime windows combined. This establishes an empirical **Nighttime Risk Premium of +1.88%** (+19.3% relative increase in accident likelihood).
- 41.0% (236 of 575) of all network crashes and 44.2% (65 of 147) of all fatalities occur during nighttime operations.

---

### D. Atmospheric Weather Multiplier Analysis

| Weather Condition | Observations | Total Volume | Mean Speed (km/h) | Total Crashes | Crash Rate (%) | Fatalities | Fatality Ratio (%) | Risk Multiplier vs. Clear |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Clear** | 1,073 | 277,644 | 61.1 | 110 | 10.25% | 38 | 34.55% | **1.00x (Baseline)** |
| **Rain** | 1,144 | 287,321 | 62.5 | 125 | 10.93% | 22 | 17.60% | **1.07x** |
| **Fog** | 1,099 | 281,001 | 60.6 | 120 | 10.92% | 28 | 23.33% | **1.06x** |
| **Snow** | 1,091 | 266,620 | 61.1 | 114 | 10.45% | 39 | 34.21% | **1.02x** |
| **Storm** | 1,093 | 284,922 | 61.3 | 106 | 9.70% | 20 | 18.87% | **0.95x** |
| **All Adverse Combined**| 4,427 | 1,119,864 | 61.4 | 465 | 10.50% | 109 | 23.44% | **1.02x** |

**Weather Insights**:
- The aggregate Adverse Weather Risk Multiplier is **1.02x**, indicating that adverse atmospheric conditions alone do not induce massive spikes in accident rate within this benchmark. Drivers appear to moderate travel behaviors slightly during severe storms (9.70% crash rate), though rain and fog elevate collision probability modestly to ~10.9%.

---

## 3. Time-Based Analysis & YoY Growth Trajectory (2018–2023)

The dataset covers 6 complete operational calendar years (2018 through 2023). Multi-year trend analysis reveals the operational impact of systemic macroeconomic mobility shifts.

### Multi-Year Operational & Safety Growth Table

| Year | Telemetry Windows | Total Volume | Mean Velocity (km/h) | Confirmed Crashes | Crash Rate (%) | Fatal Crashes | Fatality Ratio (%) | Total Alerts | Volume YoY Growth (%) | Accidents YoY Growth (%) | Velocity YoY Growth (%) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **2018** | 905 | 233,779 | 61.2 | 99 | 10.94% | 22 | 22.22% | 188 | — | — | — |
| **2019** | 907 | 229,400 | 61.8 | 98 | 10.80% | 32 | 32.65% | 180 | **-1.87%** | **-1.01%** | +0.98% |
| **2020** | 860 | 221,024 | 59.0 | 77 | 8.95% | 21 | 27.27% | 190 | **-3.65%** | **-21.43%** | -4.53% |
| **2021** | 949 | 242,981 | 63.6 | 107 | 11.28% | 21 | 19.63% | 202 | **+9.93%** | **+38.96%** | +7.80% |
| **2022** | 954 | 237,274 | 61.5 | 98 | 10.27% | 26 | 26.53% | 161 | **-2.35%** | **-8.41%** | -3.30% |
| **2023** | 925 | 233,050 | 60.5 | 96 | 10.38% | 25 | 26.04% | 182 | **-1.78%** | **-2.04%** | -1.63% |

### Historical Trajectory Insights:
1. **The 2020 Pandemic Mobility Contraction**: In 2020, total monitored vehicle volume dropped by **-3.65%** (to 221,024 vehicles) and mean velocity fell to 59.0 km/h. Concurrently, verified collisions collapsed by **-21.43%** (from 98 to 77), dropping the collision incidence rate to a multi-year low of **8.95%**.
2. **The 2021 Post-Lockdown Mobility Rebound**: In 2021, volume rebounded strongly by **+9.93%** (to 242,981 vehicles), velocity accelerated to a 6-year peak of 63.6 km/h, and collisions surged by **+38.96%** (107 crashes, 11.28% accident rate).
3. **2022–2023 Stabilization**: Over the subsequent two years, volume stabilized around 233k–237k vehicles with accident rates returning to the baseline range of 10.27%–10.38%.

---

## 4. Top & Bottom Performers

Performance extremes across the 500 network corridors highlight targeted intervention priorities. Corridors are evaluated with a minimum qualification filter of $\ge 5$ telemetry observations to eliminate low-sample noise.

### Top 5 High-Risk / Dangerous Corridors (Lethal Blackspots)

| Rank | Corridor ID | Telemetry Windows | Total Volume | Mean Velocity (km/h) | Total Collisions | Collision Rate (%) | Fatalities | Fatality Ratio (%) | Dominant Region |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **1** | **RD-137** | 9 | 2,475 | 73.4 | 4 | **44.44%** | 3 | **75.00%** | North |
| **2** | **RD-136** | 12 | 3,155 | 68.7 | 5 | **41.67%** | 0 | 0.00% | North |
| **3** | **RD-169** | 5 | 1,749 | 75.4 | 2 | **40.00%** | 0 | 0.00% | North |
| **4** | **RD-416** | 8 | 1,641 | 32.8 | 3 | **37.50%** | 0 | 0.00% | South |
| **5** | **RD-192** | 11 | 2,241 | 45.5 | 4 | **36.36%** | 1 | 25.00% | North |

*Critical Observation*: `RD-137` is the network's most lethal blackspot. Over 9 observation windows, it logged 4 collisions, **3 of which were fatal** (75% fatality ratio), accompanied by high average travel speeds (73.4 km/h).

### Top 5 Safest High-Volume Corridors (Zero-Incident Benchmarks)

| Rank | Corridor ID | Telemetry Windows | Total Volume | Mean Velocity (km/h) | Confirmed Crashes | Crash Rate (%) | Fatalities | Dominant Region |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **1** | **RD-28** | 20 | 5,604 | 54.2 | 0 | **0.00%** | 0 | South |
| **2** | **RD-406** | 16 | 5,658 | 48.6 | 0 | **0.00%** | 0 | North |
| **3** | **RD-47** | 16 | 4,898 | 60.0 | 0 | **0.00%** | 0 | North |
| **4** | **RD-104** | 16 | 3,818 | 68.9 | 0 | **0.00%** | 0 | Central |
| **5** | **RD-373** | 16 | 3,566 | 69.2 | 0 | **0.00%** | 0 | East |

*Critical Observation*: `RD-28` and `RD-406` sustained heavy cumulative vehicle volumes ($>5,600$ vehicles each across 16–20 observation windows) with **zero recorded collisions**. These corridors serve as the architectural and design benchmark for safe traffic engineering.

### Corridors by Volume Throughput Extremes

| Category | Rank | Corridor ID | Observations | Total Volume | Mean Speed (km/h) | Accidents | Accident Rate (%) |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **Highest Volume** | 1 | **RD-170** | 23 | **6,000** | 55.7 | 5 | 21.74% |
| | 2 | **RD-351** | 19 | **5,779** | 71.1 | 1 | 5.26% |
| | 3 | **RD-282** | 19 | **5,763** | 63.9 | 1 | 5.26% |
| | 4 | **RD-406** | 16 | **5,658** | 48.6 | 0 | 0.00% |
| | 5 | **RD-28** | 20 | **5,604** | 54.2 | 0 | 0.00% |
| **Lowest Volume** | 1 | **RD-489** | 2 | **140** | 36.5 | 0 | 0.00% |
| | 2 | **RD-209** | 3 | **381** | 59.7 | 1 | 33.33% |
| | 3 | **RD-189** | 5 | **727** | 64.4 | 1 | 20.00% |
| | 4 | **RD-488** | 4 | **739** | 98.2 | 0 | 0.00% |
| | 5 | **RD-105** | 4 | **739** | 64.8 | 0 | 0.00% |

---

## 5. Comprehensive Analytical Visualization Inventory

Seven analytical visualizations have been generated, styled with standardized typography and palettes, and saved to `outputs/figures/`.

| Chart Figure File | Business Question Addressed | Dataset Fields Utilized | Visual Formulation | Decision-Support Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **`kpi_executive_scorecard_bars.png`** | *How is the network performing relative to operational safety and alert benchmarks?* | `accident_flag`, `fatal_accident_flag`, `severe_accident_flag`, `alert_flag`, `road_id` | Paired horizontal benchmark bars (Actual vs Target) with green/red status encoding | Immediate executive situational awareness of performance breaches across 6 core safety ratios |
| **`kpi_yearly_volume_and_accident_trends.png`** | *How have traffic volume and collision counts evolved over the 2018–2023 multi-year horizon?* | `year`, `vehicle_count`, `accident_flag` | Dual-axis Bar (Volume) + Line (Accidents) with historical pandemic event callouts | Strategic capacity planning, multi-year trend tracking, and macro shock impact modeling |
| **`kpi_top_bottom_corridors_ranked.png`** | *Which road segments represent dangerous blackspots vs. high-volume safe corridors?* | `road_id`, `accident_flag`, `fatal_accident_flag`, `vehicle_count` | Two-panel horizontal comparative bar chart with observation and fatality annotations | Immediate capital allocation: dispatching traffic engineering and safety barriers to top 5 blackspots |
| **`kpi_regional_performance_matrix.png`** | *How do volume throughput, mean travel velocity, and collision rates vary across geographic regions?* | `region`, `vehicle_count`, `avg_speed_kmh`, `accident_flag`, `fatal_accident_flag` | 3-panel comparative bar matrix with regional volume share and speed benchmarks | Regional fleet reallocation, cross-zonal policy comparison, and regional emergency triage |
| **`kpi_modal_efficiency_and_risk.png`** | *What is the relationship between vehicle class velocities and collision/fatality risk?* | `vehicle_type`, `avg_speed_kmh`, `accident_flag`, `fatal_accident_flag` | Grouped two-panel modal velocity bar and accident vs fatality percentage comparison | Vehicle class speed restrictions, commercial fleet routing, and modal safety audits |
| **`kpi_alert_system_confusion_matrix.png`** | *What is the empirical classification accuracy and false alarm rate of the automated hazard alert system?* | `alert_flag`, `accident_flag`, `alert_success_type` | 2x2 Confusion Matrix Heatmap (TP, FP, FN, TN) with recall/precision metrics | Threshold recalibration to mitigate operator alarm fatigue (90.3% false alarms) and reduce missed crashes |
| **`kpi_daypart_safety_premium.png`** | *How does collision risk elevate during nighttime hours compared to daytime traffic windows?* | `time_of_day`, `vehicle_count`, `accident_flag` | Dual-axis Bar (Volume) + Line (Crash Rate) with Nighttime Risk Premium callout | Dynamic speed limits, enhanced nighttime roadway lighting, and nocturnal patrol scheduling |

---

## 6. Separation of Data Facts from Analytical Interpretation

To ensure integrity and prevent unfounded claims, empirical observations are rigorously separated from analytical hypotheses.

| Area | Verified Data Fact (Empirical Reality) | Analytical Interpretation (Business Hypothesis & Recommendation) |
| :--- | :--- | :--- |
| **Corridor Blackspots** | `RD-137` has an accident rate of **44.44%** and 3 fatal accidents over 9 telemetry observations with an average speed of 73.4 km/h. | High travel velocities combined with local roadway geometry or blind curves may be inducing severe crashes. Recommended action: On-site physical safety audit, speed cameras, and guardrail installation. |
| **Corridor Safety Benchmarks** | `RD-28` and `RD-406` recorded **0 collisions** across $>5,600$ vehicles monitored over 16–20 observation windows. | These corridors possess structural, signaling, or width characteristics that promote safe vehicular flow. Recommended action: Study corridor physical design as the network safety standard. |
| **Nighttime Risk** | Nighttime collision rate is **11.64%** (236 crashes, 65 fatalities) compared to **9.75%** daytime average, representing a **+1.88% risk premium**. | Reduced visibility, driver fatigue, and higher speeds in free-flowing traffic likely increase hazard rates. Recommended action: Enhanced luminaire deployment and nighttime automated speed enforcement. |
| **Automated Alerts** | 996 of 1,103 dispatched alerts were false alarms (**90.30% false alarm rate**), while only 107 of 575 crashes were preceded by an alert (**18.61% recall**). | Current alert trigger heuristics are miscalibrated, causing severe operator alarm fatigue while failing to provide predictive protection. Recommended action: Retrain threshold rules on high-density and night factors. |
| **Regional Lethality** | Central and West regions have lower crash rates ($<10\%$) but significantly higher fatality ratios (**34.25%** and **30.91%**) than the North (22.96%). | Collisions in Central and West may occur at higher open-highway velocities or in remote locations with extended ambulance arrival delays. Recommended action: Audit regional trauma response and emergency dispatch times. |
| **Pandemic Trajectory** | In 2020, monitored volume fell by **-3.65%** and total confirmed collisions fell by **-21.43%** before rebounding in 2021 by **+38.96%**. | Mobility restrictions in 2020 depressed network congestion and collision rates; lifting restrictions in 2021 caused an abrupt surge in traffic and incidents. |

---

## 7. Limitations & Documentation of Missing Business Fields

In accordance with strict professional standards, the following attributes are documented as absent from the source sensor telemetry dataset:

1. **Absence of Retail Financial Metrics**:
   - The dataset contains **no financial attributes** such as dollar sales revenue, product cost of goods sold (COGS), profit margins, invoice values, or customer accounts.
   - *Operational Replacement*: System performance is evaluated using vehicular throughput (`vehicle_count`), flow momentum (`traffic_flow_rate`), network velocity (`avg_speed_kmh`), and collision risk.
2. **Absence of Delivery and Logistics Timestamps**:
   - Fields such as shipment delivery status, on-time delivery rates, driver IDs, customer names, or freight consignment identifiers are not present.
   - *Operational Replacement*: Telemetry window timestamps (`timestamp`), operational dayparts (`time_of_day`), and corridor identifiers (`road_id`) provide operational scheduling visibility.
3. **Absence of Fuel & Energy Telemetry**:
   - Fuel consumption (liters), battery kilowatt-hour draw, fuel efficiency (km/L), and vehicle maintenance logs are absent.
   - *Operational Replacement*: Modal velocity stability and vehicle classification shares (`vehicle_type`) provide proxy visibility into fleet distribution.
4. **Synthetic Benchmark Characteristics**:
   - Minor balance in categorical distributions reflects synthetic test generation. Statistical testing is grounded strictly in the verified 5,500 record count.

---

## 8. Prioritized Roadmap for Step 07 Interactive Plotly Visualizations

The analytical static figures developed in Step 06 will be transformed into high-performance, responsive **Plotly interactive charts** in Step 07. Charts are prioritized based on user-interaction value (hover tooltips, dynamic filtering, drill-downs):

```text
Priority 1 → Executive KPI Scorecard & Visual Gauges
            - Dynamic KPI cards with baseline deltas
            - Visual threshold status gauges (Collision Rate, Recall, False Alarms)

Priority 2 → Multi-Year Time-Series & Temporal Dynamics
            - Interactive multi-year dual-axis trend line (Volume vs. Collisions) with range slider
            - Daypart collision risk comparison with hover breakdown

Priority 3 → Corridor Risk & Blackspot Exploration
            - Ranked horizontal bar chart of Top Dangerous Corridors with click-to-filter
            - Safest high-volume corridor comparative benchmarks

Priority 4 → Regional Performance & Fleet Modality Matrix
            - Multi-region comparative volume and velocity breakdown with dropdown slicers
            - Modal velocity vs incident severity interactive grouped charts

Priority 5 → Automated Hazard Alert System Diagnostics
            - Interactive 2x2 confusion matrix heatmap with drill-down into false alarm records
            - Telemetry data explorer table with column filtering and CSV export
```

---

## Conclusion

Step 06 establishes the verified analytical and visualization core of the Transportation Analytics Dashboard. All KPI logic is modularized, mathematically validated, and cleanly decoupled from UI presentation. The project is fully primed for **Step 07 — Interactive Visualization with Plotly**.
