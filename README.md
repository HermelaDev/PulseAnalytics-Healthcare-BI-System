# PulseAnalytics: A Strategic BI Framework for Hospital Readmission & Clinical Resource Optimization

## Table of Contents
1. [Problem Statement](#1-problem-statement)
2. [Project Directory](#2-project-directory)
3. [Tools & Technologies](#3-tools--technologies)
4. [Strategic Business Questions](#4-strategic-business-questions)
5. [Data Source & Characteristics](#5-data-source--characteristics)
6. [Technical Workflow](#6-technical-workflow)
    * [Data Cleaning & Transformation](#data-cleaning--transformation)
    * [Data Modeling (Star Schema)](#data-modeling-star-schema)
    * [DAX Calculations & Measures](#dax-calculations--measures)
7. [Dashboard Pages Overview](#7-dashboard-pages-overview)
8. [Key Insights](#8-key-insights)
9. [Strategic Recommendations](#9-strategic-recommendations)
10. [Project Resources](#10-project-resources)
11. [Contributors](#11-contributors)

---

## 1. Problem Statement
Hospital readmissions, particularly within 30 days of discharge, remain one of the most critical challenges in modern healthcare systems. High readmission rates contribute to increased operational costs, regulatory penalties, and compromised patient outcomes. 

This project implements an end-to-end Business Intelligence (BI) solution that transforms 10 years of U.S. hospital clinical data into actionable insights to:
* Identify high-risk patient segments.
* Reduce avoidable 30-day readmissions.
* Optimize clinical resource allocation.
* Support data-driven healthcare decision-making.

---

## 2. Project Directory
To ensure a professional and organized repository, the project is structured as follows:

| Folder/File | Description |
| :--- | :--- |
| `dashboard_images/` | Screenshots of all Power BI dashboard pages. |
| `data/` | Source dataset (`diabetic_data.csv`). |
| `Documentation/` | Technical guides: `Documented_transformations.pdf` and `Documented_modelling.pdf`. |
| `Reports/` | Final business deliverables: `Hospital_Performance_Report.pdf`. |
| `PulseAnalytics.pbix` | Core Power BI file (Data Model & Visualizations). |

---

## 3. Tools & Technologies
The following technologies were used to develop this end-to-end BI solution:

* **Microsoft Power BI:** Core platform for ETL (Power Query), Data Modeling, and interactive Data Visualization.
* **DAX (Data Analysis Expressions):** Used to develop the analytical engine, including time-intelligence and risk-scoring measures.
* **LaTeX:** Utilized to design the professional executive-style business report and technical documentation.
* **GitHub:** Employed for project version control and documentation hosting.
* **Microsoft Excel:** Used for initial data audit, metadata verification, and cross-checking transformation logic.

---

## 4. Strategic Business Questions
1. **Readmission Risk & Equity Gap:** What is the baseline 30-day readmission rate, and what is the "gap" between our highest-risk (35%) and best-performing (23%) segments?
2. **Seasonal Volume & Timing:** Are there specific months where clinical pressure spikes, necessitating resource reallocation?
3. **Clinical Intensity & Resource Load:** Is there a correlation between high-intensity interventions (medications/labs) and the likelihood of returning within 30 days?
4. **Operational Consistency:** Which primary diagnoses (e.g., Heart Failure - 428) consistently drive hospital volume and ALOS?
5. **Financial Impact:** How does the cost of care scale within our largest patient demographic, the senior population?

---

## 5. Data Source & Characteristics
* **Source:** [UCI Machine Learning Repository - Diabetes 130-US Hospitals](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)
* **Dataset Characteristics:**
    * 101,766 patient admission records.
    * 50+ clinical and administrative variables.
    * Numerical measures: Lab Procedures, Medication Count, Length of Stay.
    * Categorical attributes: Race, Gender, Specialty, Payer Code.

---

## 6. Technical Workflow

### Data Cleaning & Transformation
The raw dataset was processed using **Power Query (M)** to ensure clinical accuracy and model efficiency. Our transformation strategy focused on standardizing categorical labels and converting text-based clinical ranges into usable numeric measures.

#### **I. Core Identifiers & Demographics**
* **IDs (Columns 1 & 2):** Converted `encounter_id` and `patient_nbr` from Numbers to **Text**. This prevents accidental aggregation and keeps the data model lean.
* **Race (Column 3):** Replaced `?` placeholders with **"Other"**. Applied *Trim* and *Capitalize Each Word* for consistent demographic grouping.
* **Gender (Column 4):** Standardized casing and filtered "Unknown/Invalid" entries to ensure demographic insights are grounded in verified data.
* **Age (Column 5):** Stripped symbols `[ )`, split ranges, and calculated an **Age_Midpoint** to enable numeric DAX correlations.
* **Weight (Column 6):** Removed entirely due to 97% missingness, optimizing the model by reducing noise.

#### **II. Clinical & Operational Logic**
* **Mortality Filtering (Column 8):** Excluded `discharge_disposition_id` codes associated with expired patients (11, 13, 14, 19, 20, 21) to focus risk calculations on the at-risk living population.
* **Hospital Metrics (Columns 10-18):** Validated `time_in_hospital` and health utilization counts as **Whole Numbers** to calculate Average Length of Stay (ALOS).
* **Specialty & Payer (Columns 11-12):** Replaced `?` with **"Not Specified"**. We retained these to analyze known segments (e.g., Medicare) rather than losing valuable encounter volume.

#### **III. Diagnostics & Medication Tracking**
* **ICD-9 Codes (Columns 19-21):** Cast `diag_1`, `diag_2`, and `diag_3` as **Text** to prevent corruption of alphanumeric codes (V-codes/E-codes).
* **Lab Results (Columns 23-24):** Replaced "None" with **"Not Tested"**, accurately reflecting clinical reality versus missing data.
* **Pharmacy Load (Columns 25-47):** Bulk-validated medication dosage columns as **Text** to facilitate polypharmacy analysis.
* **Readmission Labels (Target Variable):** Transformed raw outputs into executive-ready labels:
    * `<30` → **"Under 30 Days"**
    * `>30` → **"Over 30 Days"**
    * `NO` → **"Not Readmitted"**

> **Technical Deep-Dive:** [View Documented Transformations & M-Code (PDF)](./Documentation/Documented_transformations.pdf) | [Download (PDF)](./Documentation/Documented_transformations.pdf?raw=true)

---

### Data Modeling Strategy (Star Schema)

The flat dataset was restructured into a **Star Schema**. All dimension tables were created using the **"Reference"** method in Power Query to ensure a dynamic data lineage from a single source of truth.

#### **I. The Fact Table: `Fact_Encounter`**
The core table containing quantitative measures and keys.
* **Synthetic Date Logic:** Generated a stable, unique `Synthetic_Date` for every encounter using an index-based formula, ensuring each record has a fixed point in time (1999–2008) for time-intelligence.

#### **II. Dimension Tables**
* **`Dim_Patient`:** Stores unique patient demographics. Removed duplicates to ensure a strict **1:N** relationship.
* **`Dim_Admission`:** Categorizes hospital entry/exit using **Composite Keys** (`Admission_Key`) to link complex disposition combinations.
* **`Dim_Diagnosis`:** Houses categorical ICD-9 labels to analyze how diagnosis profiles impact system risk.
* **`Dim_Date`:** A dedicated calendar table marking the official **Date Table** for the model.

#### **III. The Medication Bridge**
1.  **Unpivoting:** Flattened 24 medication columns into `Drug_Name` and `Dosage_Change_Type`.
2.  **Bridge Table (`Bridge_Medication`):** Implemented to handle the differing grains between medications and admissions.
3.  **Cross-Filtering:** Set to **"Both"** between the Fact and Bridge tables to ensure drug-specific slicers correctly filter patient encounters.

> **Architecture Deep-Dive:** [View Data Modeling & Relationship Guide (PDF)](./Documentation/Documented_modelling.pdf) | [Download (PDF)](./Documentation/Documented_modelling.pdf?raw=true)

---

###  DAX Calculations & Measures

The analytical engine of PulseAnalytics is powered by a tiered library of DAX measures. These range from foundational aggregations to complex business logic used for risk scoring and resource allocation.

#### **I. Core KPIs (Foundation Metrics)**
These measures establish the baseline volume and clinical intensity for the hospital system.

* **Total Encounters** Counts the unique admission records in the dataset.
    ```dax
    Total Encounters = DISTINCTCOUNT(Fact_Encounter[encounter_id])
    ```

* **Total Patients** Tracks the number of unique individuals treated across the 10-year period.
    ```dax
    Total Patients = DISTINCTCOUNT(Fact_Encounter[patient_nbr])
    ```

* **Average Medications per Encounter** Benchmarks the pharmacological load per hospital stay.
    ```dax
    Avg Medications per Encounter = AVERAGE(Fact_Encounter[num_medications])
    ```

* **Average Lab Procedures** Measures the diagnostic intensity of each encounter.
    ```dax
    Avg Lab Procedures = AVERAGE(Fact_Encounter[num_lab_procedures])
    ```

#### **II. Operational Intelligence**
Measures designed to monitor hospital efficiency, patient flow, and resource pressure.

* **Average Stay Duration (ALOS)** Calculates the average length of stay in days.
    ```dax
    Average Stay Duration = AVERAGE(Fact_Encounter[time_in_hospital])
    ```

* **Lab Utilization Index** Normalizes lab procedure volume against total encounters to identify high-intensity departments.
    ```dax
    Lab Utilization Index = DIVIDE(SUM(Fact_Encounter[num_lab_procedures]), [Total Encounters])
    ```

* **Medication Intensity Index** Correlates the number of medications prescribed against the number of diagnoses.
    ```dax
    Medication Intensity Index = DIVIDE(SUM(Fact_Encounter[num_medications]), SUM(Fact_Encounter[number_diagnoses]))
    ```

* **Resource Pressure Status** A dynamic status indicator that flags system strain based on lab and medication thresholds.
    ```dax
    Resource Pressure Status = 
    VAR labs = [Lab Utilization Index]
    VAR meds = [Medication Intensity Index]
    RETURN
    SWITCH(
        TRUE(),
        labs > 40 && meds > 5, "Critical",
        labs > 20, "Elevated",
        "Stable"
    )
    ```

#### **III. Advanced Analytics & Risk Scoring**
Strategic logic used to identify high-risk profiles and perform comparative analysis.

* **Readmission Risk Score** Identifies the probability of return based on previous inpatient history.
    ```dax
    Readmission Risk Score = 
    VAR HighRisk = CALCULATE(COUNT(fact_Encounter[encounter_id]), fact_Encounter[number_inpatient] > 0)
    VAR Total = CALCULATE(COUNT(fact_Encounter[encounter_id]), ALLSELECTED(fact_Encounter))
    RETURN
    DIVIDE(HighRisk, Total, 0)
    ```

* **High Utilisation Flag** Segments patients into 'High' or 'Normal' categories based on clinical resource consumption.
    ```dax
    High Utilisation Flag = 
    VAR avgMeds = [Avg Medications per Encounter]
    VAR avgLabs = [Avg Lab Procedures]
    RETURN
    IF(avgMeds > 10 || avgLabs > 40, "High", "Normal")
    ```

* **Diagnosis Rank** Ranks primary ICD-9 codes by encounter volume to identify the biggest clinical drivers.
    ```dax
    Diagnosis Rank = RANKX(ALL(Dim_Diagnosis[diag_1]), [Total Encounters], , DESC)
    ```

* **Encounter Share %** Calculates the percentage of total encounters attributed to a specific segment (e.g., gender).
    ```dax
    Encounter Share % = DIVIDE([Total Encounters], CALCULATE([Total Encounters], ALL(Dim_Patient[gender])))
    ```

#### **IV. Time Intelligence**
Measures used to track seasonal trends and growth rates across different periods.

* **Encounters YTD** Calculates Year-to-Date volume accumulation.
    ```dax
    Encounters YTD = TOTALYTD([Total Encounters], Dim_Date[Date])
    ```

* **Year-over-Year (YoY) Growth %** Measures the percentage change in volume compared to the same period in the previous year.
    ```dax
    YoY Growth % = 
    VAR LY = CALCULATE([Total Encounters], SAMEPERIODLASTYEAR(Dim_Date[Date]))
    RETURN
    DIVIDE([Total Encounters] - LY, LY)
    ```

* **Quarter-over-Quarter (QoQ) Growth %** Tracks short-term volume fluctuations.
    ```dax
    QoQ Growth % = 
    VAR LQ = CALCULATE([Total Encounters], PREVIOUSQUARTER(Dim_Date[Date]))
    RETURN
    DIVIDE([Total Encounters] - LQ, LQ)
    ```

---

## 7. Dashboard Pages Overview

#### **I. Executive Health Overview**
Tracks the baseline 33% readmission risk and total encounters.
![Executive Dashboard](dashboard_images/executive_summary.png)

#### **II. Demographic & Risk Analysis**
Identifies disparities across ethnicity, age, and gender.
![Risk & Gap Analysis](dashboard_images/readmission_analysis.png)

#### **III. Patient Journey (Drill-Through)**
Tool for viewing individual medication changes and lab results.
![Patient Drill Down](dashboard_images/patient_drillthrough.png)

---

## 8. Key Insights
* **Ethnicity Risk Disparity:** A **12% performance gap** exists between African American patients (35% risk) and Asian/Other groups (23%).
* **Seasonal Volatility:** Data reveals consistent spikes in **August and October**.
* **The "Polypharmacy" Indicator:** Patients prescribed **14+ medications** are significantly more likely to be readmitted.

> **View Comprehensive Analysis:** [View Business Performance Report (PDF)](./Reports/Hospital_Performance_Report.pdf) | [Download (PDF)](./Reports/Hospital_Performance_Report.pdf?raw=true)

---

## 9. Strategic Recommendations
* **High-Risk Daily List:** Flag patients with >10 medications for mandatory pharmacist consult.
* **Standardize Discharge:** Model protocols after the 23% risk group to close the equity gap.
* **Seasonal Staffing:** Increase administrative support during identified surges in August/October.

---

## 10. Project Resources
* **Interactive Dashboard:** [Click Here to View Power BI Public Link](#)
* **Final Presentation Slides:** [Available in Reports Folder](./Reports/Presentation_Slides.pdf) | [Download](./Reports/Presentation_Slides.pdf?raw=true)

---

## 11. Contributors

| Contributor | Primary Role | Key Responsibilities |
| :--- | :--- | :--- |
| **Van Tasi** | Lead DAX Developer | Statistical Analysis, Measure Creation, Readmission Logic |
| **Selmah Mise** | Senior Dashboard Designer | UI/UX Optimization, Visual Storytelling, Dashboard Design |
| **Peter Kidiga** | Data Engineer | ETL Transformation, Data Cleaning, Power Query |
| **Hermela Seltanu** | Data Engineer | ETL Transformation, Data Cleaning, Data Profiling |
| **Hetal Kumbharana** | Data Architect | Star-Schema Modelling, Relationship Management, Dashboard Support |