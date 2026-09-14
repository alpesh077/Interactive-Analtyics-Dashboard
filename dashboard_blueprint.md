# Transportation Analytics Dashboard — Interactive Dashboard Blueprint
**Project Milestone**: Step 05 — Business Questions & KPI Definition  
**Target Delivery**: Production Streamlit Interactive Web Application (`app.py`)  
**Architecture Style**: Multi-Tab Executive Analytics Application with Dynamic Cross-Filtering  
**Dataset Reference**: `data/processed/transportation_cleaned.parquet`  
**Status**: Approved Architecture Specification

---

## 1. Executive Dashboard Architecture & Design Principles

The Transportation Analytics Dashboard is designed to serve executive leadership, traffic operations managers, and safety engineers with an ultra-responsive, decision-focused command center.

### Core Design Principles
1. **The 30-Second Executive Rule**: An executive must understand overall network health, macro volume, safety risk, and acute problem areas within 30 seconds of opening the application.
2. **Progressive Disclosure**: High-level KPI metric cards sit at the top of each view, followed by macro visual distributions, and drillable segment-level tables at the bottom.
3. **Strict Domain Grounding**: Zero fabricated dollar numbers or artificial metrics; all visualizations reflect empirical telemetry.
4. **Dynamic Contextual Baselines**: Every filtered KPI card renders both the active slice value and a color-coded delta compared to the national network baseline.

```text
┌────────────────────────────────────────────────────────────────────────────┐
│                    GLOBAL SIDEBAR CONTROLS & FILTER BAR                    │
│  Date Range | Regional Multi-Select | Modal Vehicle Filter | Density Tier │
└─────────────────────────────────────┬──────────────────────────────────────┘
                                      │ (Dynamic Reactive State)
                                      ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                       MAIN APPLICATION WORKSPACE                           │
│  [Tab 1]      [Tab 2]       [Tab 3]       [Tab 4]       [Tab 5]    [Tab 6] │
│  Executive   Corridor Risk  Traffic Flow  Alert System  Regional   Raw Data│
│  Overview    & Hotspots     & Congestion  Reliability   & Modal    Explorer│
└────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Multi-Tab Navigation Structure & Layout Wireframes

---

### Tab 1: Executive Overview & Network Pulse
* **Target Audience**: Chief Transportation Officer, Executive Directors, Operations Leads.
* **Core Purpose**: Real-time situational awareness across volume, velocity, collision rates, and alert reliability.

```text
+----------------------------------------------------------------------------------------------------+
| TAB 1: EXECUTIVE OVERVIEW & NETWORK PULSE                                                          |
+----------------------------------------------------------------------------------------------------+
| [ KPI CARD 1 ]         | [ KPI CARD 2 ]         | [ KPI CARD 3 ]         | [ KPI CARD 4 ]          |
| Total Monitored Volume | Average Network Speed  | Confirmed Collisions   | Fatal Crash Ratio       |
| 1,397,508 Vehicles     | 61.3 km/h              | 575 (10.45% Rate)      | 147 (25.57% of Crashes) |
| Delta vs Baseline: --  | Delta vs Baseline: --  | Delta vs Baseline: --  | Delta vs Baseline: --   |
+----------------------------------------------------------------------------------------------------+
| CALLOUT ALERT BANNER:                                                                              |
| ⚠️ Acute Safety Hazard: Corridor RD-137 exhibits 44.4% crash rate & 33.3% fatality rate.          |
+----------------------------------------------------------------------------------------------------+
| [ ROW 1 - LEFT 60% ]                                    | [ ROW 1 - RIGHT 40% ]                    |
| Multi-Year Throughput & Collision Trajectory (2018-2023)| Regional Volume Share (%)                |
| Plotly Dual-Axis: Total Volume (Bars) vs Crashes (Line) | Plotly Donut / Bar Chart: North (31.2%), |
| Annotated: 2020 Mobility Restrictions Dip               | South (26.3%), East (17.3%), etc.        |
+----------------------------------------------------------------------------------------------------+
| [ ROW 2 - FULL WIDTH ]                                                                             |
| Operational Daypart Throughput vs. Collision Probability                                           |
| Dual-Axis Grouped Chart: Morning Peak vs Midday vs Evening Peak vs Nighttime Peak Risk (11.64%)    |
+----------------------------------------------------------------------------------------------------+
```

---

### Tab 2: Corridor Risk & Lethal Hotspots
* **Target Audience**: Corridor Safety Engineers, Highway Maintenance Directors, Police Dispatch.
* **Core Purpose**: Precision blackspot detection and infrastructure countermeasure targeting.

```text
+----------------------------------------------------------------------------------------------------+
| TAB 2: CORRIDOR RISK & LETHAL HOTSPOTS                                                             |
+----------------------------------------------------------------------------------------------------+
| [ KPI CARD 1 ]         | [ KPI CARD 2 ]         | [ KPI CARD 3 ]         | [ KPI CARD 4 ]          |
| Monitored Corridors    | Acute Blackspots       | Lethal Clusters        | Highest Risk Corridor   |
| 500 Highway Segments   | 7 Corridors (Rate>=20%)| 3 Corridors (Fatal>=2) | RD-137 (44.4% Crash)    |
+----------------------------------------------------------------------------------------------------+
| [ ROW 1 - LEFT 50% ]                                    | [ ROW 1 - RIGHT 50% ]                    |
| Top 10 Most Dangerous Corridors (Ranked Horizontal Bar) | Corridor Velocity vs Collision Risk      |
| RD-137, RD-170, RD-136, RD-400, RD-450, RD-192...       | Interactive Plotly Scatter: Volume vs    |
| Stacked by: Fatal, Major, Minor, Unspecified            | Accident Rate, Sized by Severe Crashes   |
+----------------------------------------------------------------------------------------------------+
| [ ROW 2 - FULL WIDTH ]                                                                             |
| Interactive High-Risk Corridor Audit Table                                                         |
| Searchable, sortable table: Road ID | Obs Count | Accidents | Fatal | Severe | Crash Rate | Actions |
+----------------------------------------------------------------------------------------------------+
```

---

### Tab 3: Traffic Flow & Congestion Dynamics
* **Target Audience**: Traffic Flow Managers, Urban Highway Authorities.
* **Core Purpose**: Understand congestion propagation, bottleneck density, and velocity health.

```text
+----------------------------------------------------------------------------------------------------+
| TAB 3: TRAFFIC FLOW & CONGESTION DYNAMICS                                                          |
+----------------------------------------------------------------------------------------------------+
| [ KPI CARD 1 ]         | [ KPI CARD 2 ]         | [ KPI CARD 3 ]         | [ KPI CARD 4 ]          |
| Mean Traffic Flow Rate | Free-Flow Corridors    | Very High Density Share| Optimal Velocity Share  |
| 15,558.4 Momentum      | 248 Corridors (>60km/h)| 1,358 Obs (24.7%)      | 52.4% within 55-65 km/h |
+----------------------------------------------------------------------------------------------------+
| [ ROW 1 - LEFT 50% ]                                    | [ ROW 1 - RIGHT 50% ]                    |
| Velocity Distribution Across Density Tiers              | Traffic Density Score Breakdown          |
| Plotly Boxplot: Low vs Medium vs High vs Very High      | Plotly Funnel/Bar: Low (25.9%), Medium   |
| Evaluating Speed Compression Under Congestion           | (25.3%), High (24.2%), Very High (24.7%) |
+----------------------------------------------------------------------------------------------------+
| [ ROW 2 - FULL WIDTH ]                                                                             |
| Diurnal Hourly Traffic Velocity & Volume Heatmap                                                   |
| Hour of Day (0-23) vs Day of Week (Mon-Sun) displaying Average Speed / Flow Momentum               |
+----------------------------------------------------------------------------------------------------+
```

---

### Tab 4: Hazard Alert System Performance & Optimization
* **Target Audience**: Automated Safety Systems Engineers, Telemetry Architects, Dispatch Leads.
* **Core Purpose**: Audit warning sensitivity, diagnose false-alarm fatigue, and tune trigger thresholds.

```text
+----------------------------------------------------------------------------------------------------+
| TAB 4: HAZARD ALERT SYSTEM PERFORMANCE & OPTIMIZATION                                              |
+----------------------------------------------------------------------------------------------------+
| [ KPI CARD 1 ]         | [ KPI CARD 2 ]         | [ KPI CARD 3 ]         | [ KPI CARD 4 ]          |
| Warnings Dispatched    | Alert Sensitivity      | False Alarm Overhead   | Missed Incident Rate    |
| 1,103 Total Alerts     | 18.61% Recall (Low)    | 90.30% False Alarms    | 81.39% Unwarned Crashes |
| 20.05% Dispatch Rate   | Target: > 75.0%        | Target: < 25.0%        | 468 Crashes with Zero   |
+----------------------------------------------------------------------------------------------------+
| [ ROW 1 - LEFT 50% ]                                    | [ ROW 1 - RIGHT 50% ]                    |
| Alert Classification Matrix (Confusion Distribution)    | Warning Sensitivity by Operational Zone  |
| Plotly Bar: True Positives (107), False Alarms (996),   | Alert Recall across North, South, East,  |
| Missed Incidents (468), Normal Flow (3,929)             | West, Central and Dayparts               |
+----------------------------------------------------------------------------------------------------+
| [ ROW 2 - FULL WIDTH ]                                                                             |
| Alert System Optimization Recommendation Engine                                                    |
| Multi-factor rule proposals to re-train the warning algorithm combining Visibility, Density & Speed|
+----------------------------------------------------------------------------------------------------+
```

---

### Tab 5: Regional & Modal Slicing
* **Target Audience**: Regional Directors, Commercial Fleet Regulators, Policy Planners.
* **Core Purpose**: Cross-regional comparative benchmarking and commercial fleet modal comparisons.

```text
+----------------------------------------------------------------------------------------------------+
| TAB 5: REGIONAL & MODAL SLICING                                                                    |
+----------------------------------------------------------------------------------------------------+
| [ ROW 1 - 5-REGION COMPARATIVE BENCHMARK SCORECARDS ]                                              |
| North: 436k Vol | 11.4% Crash  |  South: 368k Vol | 11.1% Crash  |  East: 242k Vol | 9.4% Crash   |
| Central: 208k Vol | 9.1% Crash |  West: 144k Vol | 9.8% Crash    |                             |
+----------------------------------------------------------------------------------------------------+
| [ ROW 2 - LEFT 50% ]                                    | [ ROW 2 - RIGHT 50% ]                    |
| Modal Velocity vs Collision Incidence Grouped Bar       | Regional Accident Severity Profiles      |
| Bikes vs Buses vs Cars vs Trucks vs Mixed               | Stacked 100% Bar: Fatal, Major, Minor,   |
| Comparing Speed, Volume, and Collision Probability      | and Unspecified distribution by Region   |
+----------------------------------------------------------------------------------------------------+
```

---

### Tab 6: Telemetry Data Explorer & Raw Records Audit
* **Target Audience**: Data Quality Engineers, Forensic Analysts, Regulatory Compliance Officers.
* **Core Purpose**: Direct interactive query access, multi-field filtering, and secure export.

```text
+----------------------------------------------------------------------------------------------------+
| TAB 6: TELEMETRY DATA EXPLORER & RAW RECORDS AUDIT                                                 |
+----------------------------------------------------------------------------------------------------+
| Interactive Filter Widgets: Select Specific Road ID | Severity Grade | Alert Status | Date Window   |
| Search Bar: Text search by Record ID or Corridor ID                                                |
+----------------------------------------------------------------------------------------------------+
| Cleaned Telemetry Data Table (Paginated / Virtualized Streamlit DataFrame)                          |
| Columns: Record ID | Timestamp | Region | Road ID | Volume | Speed | Weather | Crash | Severity... |
+----------------------------------------------------------------------------------------------------+
| Export Controls: [ 📥 Download Filtered CSV ]   [ 📥 Download Filtered Parquet ]                   |
+----------------------------------------------------------------------------------------------------+
```

---

## 3. Executive Insights: The 30–60 Second Scan

When an executive opens the dashboard, the following 5 critical answers are delivered immediately on Tab 1 without requiring manual configuration:

1. **How is the network performing?**
   > *Network is operating at normal velocity ($61.3\text{ km/h}$) with steady multi-year volume ($1.4\text{M}$ vehicles), but sustains a persistent $10.45\%$ base collision rate ($575$ total crashes).*
2. **What changed?**
   > *Following the 2020 mobility restriction dip ($221\text{k}$ volume, $77$ crashes), traffic rebounded strongly in 2021 ($243\text{k}$ volume, $107$ crashes) and has stabilized through 2023.*
3. **Where is risk concentrated?**
   > *Geographically, the **Northern** ($11.36\%$) and **Southern** ($11.08\%$) regions account for the highest collision probabilities. Temporally, **Nighttime** hours ($22:00 - 07:00$) experience $41\%$ of all crashes with an elevated $11.64\%$ collision rate.*
4. **What acute issue requires immediate attention?**
   > *Corridor **`RD-137`** is an acute blackspot with a **$44.4\%$ collision rate and $33.3\%$ fatality rate** ($3$ deaths in $9$ passes), representing the single highest safety priority in the network.*
5. **What operational system is failing?**
   > *The automated hazard warning system has a **$90.3\%$ false-alarm rate** and misses **$81.4\%$ of actual collisions**, demanding an immediate algorithmic overhaul.*

---

## 4. Technical Architecture & Streamlit State Management

```text
Streamlit Runtime Pipeline
│
├── 1. Ingestion Layer
│   └── src.data_loader.load_cleaned_data() (Cached using @st.cache_data)
│
├── 2. Reactive State Layer
│   ├── User Sidebar Inputs (Date, Region, Vehicle Type, Density)
│   └── Sliced DataFrame Generation (Filtered in memory)
│
├── 3. Analytics & KPI Engine
│   ├── src.kpi.compute_executive_transport_kpis(df_filtered, baseline_df=df_raw)
│   └── src.analysis.analyze_corridor_safety(df_filtered)
│
└── 4. Presentation & Visualization Layer
    ├── KPI Scorecard Cards with Deltas
    ├── Interactive Plotly Charts (Themed with Corporate Navy #1E3A8A & Teal #0D9488)
    └── Download Handlers for CSV and Parquet
```

This blueprint establishes the approved structural foundation for **Step 06 — KPI Calculation & Analytical Visualization**.
