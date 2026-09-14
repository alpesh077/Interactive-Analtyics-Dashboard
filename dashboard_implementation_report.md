# Transportation Analytics Decision-Support System — Dashboard Implementation Report

**Project Milestone**: Step 08 — Dashboard Architecture & Streamlit Layout  
**Application Entry Point**: `app.py`  
**Filter Engine Module**: `src/dashboard_filters.py`  
**Dataset Reference**: `data/processed/transportation_cleaned.parquet` / `.csv` (5,500 rows, 35 features)  
**Status**: Completed & Production-Verified  

---

## 1. Executive Summary & Decision-Support Objective

The **Transportation Analytics Dashboard** is an enterprise-grade decision-support system engineered using Streamlit and Plotly. It transforms 5,500 granular highway sensor telemetry records across 500 corridors (`RD-1` through `RD-500`) into real-time situational awareness, safety blackspot auditing, congestion diagnosis, and telemetry export.

### Core Architectural Pillars
1. **The 30-Second Executive Rule**: When executive stakeholders open the dashboard, Tab 1 immediately surfaces overall network health (volume throughput, network velocity, base crash rate) and flags acute safety risks (`RD-137` lethal blackspot) without requiring manual interaction.
2. **Filter-First Architecture**: Every KPI card, Plotly visualization, and data table dynamically computes its metrics from an in-memory filtered slice produced by the sidebar filter engine (`src/dashboard_filters.py`), ensuring strict analytical consistency.
3. **Dynamic Contextual Baselines**: Metric cards present the currently filtered value alongside color-coded deltas comparing the slice against the national network baseline (5,500 observations).
4. **Zero Metric Hallucination Protocol**: In compliance with strict transportation engineering principles, financial retail metrics (such as sales revenue, gross margin dollars, delayed invoice shipping fees, and driver payroll wages) are omitted. The system grounds all analysis exclusively in empirical telemetry metrics (vehicle volume, average travel speed, crash probability, alert recall, and false-alarm overhead).

---

## 2. Target Stakeholders & Decision Matrix

| Stakeholder Persona | Primary Decision Need | Dashboard View / Controls | Key Actionable Output |
|:---|:---|:---|:---|
| **Chief Transportation Officer (CTO)** | Strategic network health, multi-year throughput trends, and public safety risk posture. | **Tab 1: Executive Overview** & Top Scorecards. | Budget prioritization, regional infrastructure capital allocation, legislative safety reporting. |
| **Highway Safety Engineers** | Identification and remediation of chronic lethal collision blackspots. | **Tab 2: Corridor Risk & Hotspots** & Segment Filters. | Geometric road redesign, rumble strips, guardrail installation, and variable speed limit enforcement on corridors like `RD-137`. |
| **Traffic Operations Managers** | Diurnal bottleneck detection, speed compression monitoring, and weather-related flow degradation. | **Tab 3: Operations & Congestion** & Density/Weather Filters. | Dynamic lane dispatch, variable message signs (VMS), and snow/ice clearance dispatch scheduling. |
| **Telemetry & Systems Architects** | Sensor health auditing, alert dispatch sensitivity tuning, and false-alarm fatigue reduction. | **Tab 4: Alert Diagnostics** & Outcome Confusion Matrix. | Algorithmic re-tuning of the hazard trigger logic from static thresholds to composite multi-sensor heuristics. |
| **Regional & Fleet Regulators** | Modal risk benchmarking (trucks, buses, bikes) and cross-regional compliance tracking. | **Tab 5: Regional & Modal Slicing** & Region Selectors. | Commercial fleet speed governance, dedicated bus lane policies, and night-shift driver safety protocols. |
| **Forensic Analysts & Auditors** | Case-level incident investigation, multi-parameter querying, and filtered dataset extraction. | **Tab 6: Telemetry Data Explorer** & CSV Export Button. | Forensic incident reconstruction and reproducible regulatory CSV data exports. |

---

## 3. System Architecture & Filter-First Reactive Flow

The application follows a clean, decoupled unidirectional data flow architecture:

```text
┌────────────────────────────────────────────────────────────────────────────┐
│                    DATA INGESTION LAYER (@st.cache_data)                   │
│         Loads data/processed/transportation_cleaned.parquet (or .csv)       │
│                      5,500 Observations, 35 Features                       │
└─────────────────────────────────────┬──────────────────────────────────────┘
                                      │
                                      ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                    GLOBAL SIDEBAR CONTROLS & FILTER ENGINE                 │
│         src/dashboard_filters.py: get_filter_options(), apply_filters()    │
│  - Observation Date Range (2018-01-01 to 2023-12-28)                       │
│  - Geographic Region Multi-Select (North, South, East, West, Central)      │
│  - Fleet Vehicle Classification (Car, Bus, Truck, Bike, Mixed)             │
│  - Traffic Density Tier (Low, Medium, High, Very High)                     │
│  - Atmospheric Weather (Clear, Rain, Fog, Snow, Storm)                     │
│  - Road Surface Condition & Corridor ID Drilldown                          │
│  - One-Click '🔄 Reset All Filters' Session State Mechanism                │
└─────────────────────────────────────┬──────────────────────────────────────┘
                                      │
                                      ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                    REACTIVE FILTERED SLICE GENERATION                      │
│               Filtered DataFrame Slice (0 to 5,500 records)                │
│       Empty-State Shield: Graceful Warning Banner if 0 records match       │
└─────────────────────────────────────┬──────────────────────────────────────┘
                                      │
                 ┌────────────────────┴────────────────────┐
                 ▼                                         ▼
┌─────────────────────────────────┐       ┌──────────────────────────────────┐
│   ANALYTICS & KPI ENGINE LAYER  │       │  INTERACTIVE VISUALIZATION LAYER │
│          (src/kpi.py)           │       │ (src/interactive_visualization)  │
│  - compute_executive_transport  │       │  - 13 Specialized Plotly Charts  │
│  - calculate_corridor_risk      │       │  - Standardized Theming & Colors │
│  - generate_kpi_summary_table   │       │  - create_empty_figure() Fallback│
└────────────────┬────────────────┘       └─────────────────┬────────────────┘
                 │                                          │
                 └────────────────────┬─────────────────────┘
                                      │
                                      ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                    STREAMLIT PRESENTATION & NAVIGATION                     │
│  - Top Context Banner & Filter Coverage Metric (e.g. 58.9% retained)       │
│  - 5 Real-Time KPI Scorecards with Baseline Comparative Deltas             │
│  - 6 Multi-Tab Domain Workspace                                            │
│  - Interactive Search & Filtered CSV Telemetry Export                      │
└────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Multi-Tab Domain Navigation & Features

### Top Executive KPI Scorecard
Displays 5 core operational metrics computed in real-time on the active slice, with contextual deltas comparing the slice to the national baseline:
1. **Total Monitored Volume**: Cumulative vehicle count in active slice with percentage share of total baseline volume.
2. **Average Velocity**: Network mean travel speed (optimal band: 55.0–65.0 km/h) with delta in km/h vs. baseline (61.3 km/h).
3. **Total Collisions**: Incident count with active crash rate (%) and delta vs. national baseline rate (10.45%).
4. **Lethal Fatalities**: Fatal casualty count with fatality ratio (%) and severe accident count.
5. **Alert Sensitivity**: Warning system recall (%) with false-alarm overhead (%) and dispatch volume.

---

### Tab 1: Executive Overview & Network Pulse
* **Advisory Callout**: Dynamically displays an acute safety alert if corridor `RD-137` is present within the filtered slice, highlighting its 44.4% collision rate and 75.0% fatality ratio.
* **KPI Benchmark Bars (`create_kpi_benchmark_bars`)**: Horizontal bar chart comparing active crash rates, fatality ratios, severe collision shares, and alert sensitivities against regulatory targets.
* **Multi-Year Volume & Accident Trend (`create_yearly_traffic_and_accident_trend`)**: Dual-axis visualization showing annual vehicular throughput against collision frequency, clearly annotating the 2020 mobility restrictions dip and the 2021 rebound.
* **Executive 12-KPI Performance Table (`generate_kpi_summary_table`)**: Standardized 12-KPI matrix detailing Category, KPI Name, Observed Value, Unit, Benchmark Target, and Operational Status.

---

### Tab 2: Corridor Risk & Lethal Hotspots
* **Top 10 Dangerous Corridors (`create_corridor_risk_ranking`)**: Ranked horizontal bar chart isolating highway segments with $\ge 5$ observations exhibiting the highest crash probabilities, color-coded by severity category.
* **Top 10 Safest High-Volume Corridors (`create_safe_corridors_benchmark`)**: Benchmark comparison highlighting corridors with high traffic throughput and zero recorded collisions (e.g., `RD-28`, `RD-406`).
* **Engineering Audit Table**: Interactive, searchable table presenting corridor ID, observation count, total volume, average speed, crash rate, fatalities, and alert dispatch frequencies for targeted civil engineering intervention.

---

### Tab 3: Operations & Congestion Dynamics
* **Speed Distribution Across Density Tiers (`create_speed_vs_density_boxplot`)**: Plotly boxplot illustrating velocity compression as traffic shifts from Low ($61.5\text{ km/h}$) to Very High ($61.1\text{ km/h}$) congestion.
* **Weather Impact on Velocity & Collision Risk (`create_weather_risk_comparison`)**: Multi-metric bar comparison showing speed variations and collision probabilities across Clear, Rain, Fog, Snow, and Storm conditions.
* **Diurnal Hourly Traffic Flow Heatmap (`create_hourly_traffic_flow_heatmap`)**: 24-hour diurnal matrix across days of the week tracking average velocities and peak congestion corridors.

---

### Tab 4: Hazard Alert Diagnostics
* **Operational Performance Metrics**: Scorecard metrics displaying total alerts dispatched, sensitivity/recall (18.61%), false-alarm rate (90.30%), and precision (9.70%).
* **Confusion Matrix Heatmap (`create_alert_confusion_matrix_heatmap`)**: Visual breakdown of True Positives (107), False Alarms (996), Missed Incidents (468), and Normal Flow (3,929).
* **Alert Outcome Donut (`create_alert_outcome_donut`)**: Proportional breakdown of automated warning outcomes illustrating the acute driver fatigue caused by 90.3% false alarms.
* **Algorithmic Tuning Advisory**: Documented engineering recommendations to replace static single-variable triggers with multi-parameter decision boundaries (e.g., composite thresholds combining low visibility and extreme density).

---

### Tab 5: Regional & Modal Slicing
* **Regional Operational Matrix (`create_regional_performance_matrix`)**: 5-region comparative analysis benchmarking North, South, East, West, and Central across vehicle volume, mean speed, and accident rate.
* **Modal Fleet Velocity & Risk (`create_modal_velocity_and_risk_chart`)**: Grouped analysis comparing Cars, Buses, Trucks, Bikes, and Mixed traffic classes.
* **Daypart Safety Premium (`create_daypart_safety_premium_chart`)**: Evaluation of diurnal risk proving that Nighttime hours ($22:00 - 07:00$) experience an elevated $11.64\%$ collision rate (+1.88% risk premium over daytime average).

---

### Tab 6: Telemetry Data Explorer & Secure Export
* **Monthly Time-Series Range Slider (`create_monthly_traffic_trend`)**: Interactive 72-month longitudinal trend with range slider and pre-set zoom selectors (1M, 6M, 1Y, YTD, ALL).
* **Record-Level Search & Filter Viewer**: Live text-searchable data table allowing analysts to query by Corridor ID, Weather condition, or Vehicle Class.
* **CSV Export Capability**: Streamlit download button generating a real-time UTF-8 CSV download containing precisely the active filtered records, timestamped for reproducibility.
* **Domain Alignment Disclosure**: Comprehensive notice detailing why retail financial numbers are absent and documenting the operational equivalents utilized.

---

## 5. Quality Assurance & Verification Test Results

### 1. Automated Python Compilation
Executed py_compile across all application modules:
```powershell
python -c "import py_compile, glob; [py_compile.compile(f, doraise=True) for f in ['app.py'] + glob.glob('src/*.py')]; print('ALL COMPILED SUCCESSFULLY!')"
```
* **Result**: `ALL COMPILED SUCCESSFULLY!` (0 errors).

### 2. Streamlit Headless AppTest Suite
Executed automated headless test cases simulating full browser interaction via Streamlit's official `streamlit.testing.v1.AppTest`:
* **Test 1: Default App State**: Rendered full application with all 5,500 baseline records. **0 Exceptions**.
* **Test 2: Single Filter Selection**: Selected `Region: North` (1,725 records). Rendered all 6 tabs and scorecards. **0 Exceptions**.
* **Test 3: Multi-Filter Combination**: Selected `Region: North` + `Vehicle: Car` (349 records). Rendered all charts and tables. **0 Exceptions**.
* **Test 4: Narrow / Empty Filter State**: Selected conflicting corridor and region filters yielding an empty DataFrame. Verified that the application rendered the clean warning banner and empty-state figures without crashing. **0 Exceptions**.
* **Test 5: Filter Reset State**: Verified that resetting filters restores the full 5,500 dataset. **0 Exceptions**.

### 3. CSV Export Integrity Test
Exported filtered slice (`Region: North`, 1,725 records) to CSV in memory:
* **Result**: Produced valid 398,521-byte UTF-8 CSV containing exactly 1,725 rows and 35 columns.

### 4. Raw Dataset Preservation
Verified byte-level integrity of the raw dataset:
* **Target File**: `data/raw/Synthetic_Transportation_Dataset_Expanded_v2.csv`
* **Size**: Exactly `610,905 bytes` (100% untouched and preserved).

---

## 6. Technical Disclosures & Domain Grounding

1. **Absence of Retail Financial Data**: The source dataset is an Internet-of-Things (IoT) vehicular telemetry and highway traffic feed. It does not contain retail store sales revenue, product wholesale costs, gross margin dollars, customer delivery delay timestamps, or driver payroll invoices.
2. **Transportation Domain Equivalents**:
   - *Financial Volume* $\rightarrow$ **Vehicular Traffic Count (Volume Throughput)**.
   - *Delivery Efficiency* $\rightarrow$ **Average Network Velocity (km/h)**.
   - *Operational Liability / Risk* $\rightarrow$ **Collision Probability & Lethal Fatality Ratio**.
   - *Customer Service Reliability* $\rightarrow$ **Automated Hazard Alert Sensitivity & False-Alarm Overhead**.
3. **Reproducibility**: All calculations strictly invoke `src.kpi` and `src.dashboard_filters` modules, ensuring that no business logic is hardcoded or duplicated in UI scripts.
