# 🔬 Automated Catalytic Performance Analytics Pipeline (DRM)

## 📌 Executive Summary & Project Purpose
In heterogeneous catalysis laboratories, researchers generate massive volumes of unaligned, multi-format time-series data from mass spectrometers and reactor plant logging software. Traditionally, scientists spend hours manually cutting, aligning, interpolating, and processing these files inside Excel spreadsheets, a method that is highly repetitive, slow, and prone to human error.

This project completely eliminates manual spreadsheet data handling by introducing an **Automated Data Engineering & Analytics Pipeline** written in Python. The script programmatically scans a target local directory, dynamically pairs mismatched raw operational text files, executes time-series nearest-neighbor timeline alignments (`pd.merge_asof`), calculates complex chemical engineering Key Performance Indicators (KPIs), and instantly outputs enterprise-ready analytics. 

By leveraging this programmatic approach, a laboratory can scale its screening throughput seamlessly, reducing hours of data crunching down to a **single-click execution.**

## 🚀 Core Features & Automation Logic
* **No Hardcoding Required:** The pipeline handles an unlimited number of catalytic experiments dynamically. It acts as an intelligent folder scanner that detects new datasets automatically.
* **Smart File Reconnaissance:** As long as raw data text files follow a predictable nomenclature prefix, the engine automatically pairs them up. For any given identifier `[ID]`, it isolates:
  * `[ID]_bypass.dat` — Calibrated baseline inlet feed gas metrics.
  * `[ID]_reaction.dat` — Outlet gas mass spectrometry concentrations.
  * `[ID]_parameters.dat` — High-frequency reactor plant which tracks temperatures, pressure, flowrates, humidity and etc.
* **Irregular Timeline Alignment:** Because the gas analytics machine and the reactor temperature sensors record timestamps at different intervals, the script runs a time-series interpolation and a `merge_asof` (nearest-neighbor) join to map the temperature values directly onto the exact timeline grid of the reaction file without losing a single real gas measurement.
* **Calibrated Concentration KPI Calculations:** The software reads calibrated mol% gas streams, calculates baseline averages from the bypass runs, and runs drift-free chemical formulas for:
  * **Conversions (%)** for Methane (CH4) and Carbon Dioxide (CO2).
  * **Yields (%)** for Hydrogen (H2), Carbon Monoxide (CO), and Water Vapor (H2O) byproduct.
  * **Selectivities (%)** for H2 and CO as desired products
  * **Syngas Product Quality** (H2/CO molar ratios).
  * **Mass Conservation Diagnostics** via a complete gas-phase Carbon Balance profile.
  * **Long Term Stability** via tracking of CO yields over time to access the deactivation kinetics

## 🗂️ Pipeline Outputs & Multi-Dashboard BI Architecture
The system delivers a comprehensive multi-layered data asset architecture designed for both localized debugging and cloud-hosted enterprise storytelling:

### 1. Multi-Sheet Structured Excel Workbooks
For every processed catalyst experiment, the script automatically exports a structured `.xlsx` workbook split into three clean domains:
* **`Sheet1_Combined_Data`**: The unified master timeline matrix matching catalytic reaction data with interpolated reactor plant temperatures, flowrates etc.
* **`Sheet2_Bypass_Raw`**: The un-reacted raw feed baselines.
* **`Sheet3_Calculated_KPIs`**: An isolated table containing only the calculated metrics (Conversion, yields etc) over time for rapid future processing with other softwares e.g Origin.

### 2. Python Plotly Interactive Dashboard
The script outputs an interactive, 6-row comparative line visual grid directly in the workspace. To maximize the user experience, legend groups are synchronized; clicking a single catalyst toggles all its respective lines across subplots simultaneously. It provides:
* **Row 1:** CH4 & CO2 Conversions vs. Temperature.
* **Row 2:** H2, CO, and H2O Yield Matrix vs. Temperature.
* **Row 3:** Syngas Product Quality (H2/CO Ratio) vs. Temperature.
* **Row 4:** Product Selectivities vs. Temperature.
* **Row 5:** Gas-Phase Carbon Balance vs. Temperature *(used to visually diagnose active carbon coking/soot deposition thresholds when the metric drops below 100%)*.
* **Row 6:** Long-Term Stability deactivation curves over time (Minutes), featuring a secondary Y-axis layout that embeds exact temperatures into a consolidated, clean 3-in-1 interactive hover card.

### 3. Consolidated Master Dataset
To provide long-term reporting flexibility, the script aggregates all processed catalyst rows vertically into a single long-format flat text file: **`Master_DRM_LookerStudio_Data.csv`**. This file includes an explicit `Catalyst_ID` column on every row and is optimized for direct ingestion into cloud BI software like **Tableau** and **Google Looker Studio**, enabling executives to implement global dropdown menu slicers that filter every chart across the canvas instantly with zero backend code required.

## 🛠️ Repository File Structure
```text
├── All data project/               # Local laboratory data repository
│   ├── Catalyst_01_bypass.dat      # Raw MS inlet gas metrics
│   ├── Catalyst_01_reaction.dat    # Raw MS reaction gas metrics
│   ├── Catalyst_01_parameters.dat  # Raw reactor plant thermal log text
│   └── ...                         # Supporting catalyst iterations (Cat 04, Cat 05, etc.)
├── DRM_Master_Pipeline.ipynb       # Main Jupyter Notebook housing the automated data engine
├── README.md                       # Repository Documentation file
└── Master_DRM_LookerStudio_Data.csv # Unified target output dataset generated for Tableau/Looker Studio ingestion
```
