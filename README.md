# PulseAnalytics: A Strategic BI Framework for Reducing Hospital Readmission & Optimizing Clinical Resource Allocation


## 1. Problem Statement

Hospital readmissions particularly within 30 days of discharge remain one of the most critical challenges in modern healthcare systems. High readmission rates contribute to increased operational costs, regulatory penalties, and compromised patient outcomes.

Despite the availability of extensive clinical data, healthcare administrators often lack real-time analytical visibility into the operational and clinical drivers of readmission risk. Factors such as medication intensity, laboratory utilization, discharge disposition, payer category, and patient demographics are rarely analyzed within a unified framework.

This project implements an end-to-end Business Intelligence (BI) solution that transforms 10 years of U.S. hospital clinical data into actionable insights. The objective is to enable hospital leadership to:

- Identify high-risk patient segments  
- Reduce avoidable 30-day readmissions  
- Optimize clinical resource allocation  
- Improve ward-level operational efficiency  
- Support data-driven healthcare decision-making  

### Project Directory

To ensure a professional and organized repository, the project is structured as follows:
* **dashboard_images/**: Screenshots of all Power BI dashboard pages.
* **Data/**: Contains `diabetic_data.csv` (Source dataset).
* **Documentation/**: Technical guides including `Documented_transformations.pdf` and `Documented_modelling.pdf`.
* **Reports/**: Final business deliverables, including the `Hospital_Performance_Report.pdf`.
* **PulseAnalytics-BI-Project.pbix**: The core Power BI file containing the data model and visualizations.

## 2. Strategic Business Questions

### 1. Readmission Risk & Equity Gap
What is the baseline 30-day readmission rate, and how do rates vary across different ethnicities and age groups? Specifically, what is the "gap" between our highest-risk and best-performing segments?

### 2. Seasonal Volume & Timing
Are hospital return rates consistent throughout the year, or are there specific months where clinical pressure spikes, necessitating resource reallocation?

### 3. Clinical Intensity & Resource Load
Is there a correlation between high-intensity clinical interventions (number of medications prescribed and laboratory procedures performed) and the likelihood of a patient returning within 30 days?

### 4. Operational Consistency & Diagnosis Drivers
Which primary diagnoses (ICD-9 codes) consistently drive hospital volume and length of stay (ALOS)? Does this pattern remain stable across multiple years of data?

### 5. Financial Impact & Revenue Drivers
Which clinical categories contribute most significantly to diagnostic revenue, and how does the cost of care (pharmacy and lab utilization) scale within our largest patient demographic, the senior population?



## 3. Data Source & Justification

### Data Source
- UCI Machine Learning Repository
- Kaggle

### Dataset
**Diabetes 130-US Hospitals Dataset (1999–2008)**

```

https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008

```

### Dataset Characteristics
- 101,766 patient admission records  
- 50+ clinical and administrative variables  
- Admission and discharge date fields  
- Categorical attributes (Race, Gender, Specialty, Payer Code)  
- Numerical measures (Lab Procedures, Medication Count, Length of Stay)  

This dataset exceeds the minimum 8,000-row requirement and provides sufficient structural complexity for enterprise-grade BI implementation.



### B. Data Modeling Strategy (Star Schema)

The flat dataset will be restructured into a Star Schema consisting of:

#### Fact Table
**Fact_Admissions**
- Admission ID
- Patient Key
- Specialty Key
- Date Key
- Lab Procedure Count
- Medication Count
- Length of Stay
- Readmission Indicator
- Derived Cost Measure

#### Dimension Tables
- **Dim_Patients** (Age, Gender, Race)
- **Dim_Specialties**
- **Dim_Payer**
- **Dim_Discharge**
- **Dim_Date** (Dedicated Date table built using DAX)

The model will implement:
- One-to-many relationships
- Proper cardinality
- Single-direction filtering
- Clean and professional model view

### 4. Dashboard Pages Overview

Our Power BI solution is structured into four strategic layers to provide both executive summaries and granular operational data:

#### **I. Executive Health Overview**
A high-level "Command Center" for hospital leadership. This page tracks the baseline 33% readmission risk, total patient encounters, and unique patient counts across the 10-year dataset.

![Executive Dashboard](dashboard_images/executive_summary.png)

#### **II. Demographic & Risk Analysis**
A deep-dive into patient segments to identify health disparities. This page allows administrators to filter by ethnicity, age, and gender to pinpoint where the 30-day return rate deviates from the system average.

![Risk & Gap Analysis](dashboard_images/readmission_analysis.png)

#### **III. Clinical & Operational Efficiency**
This page analyzes resource utilization, mapping the Average Length of Stay (ALOS) against primary diagnoses and tracking the volume of lab procedures and medications per encounter.

![Operational Dashboard](dashboard_images/operations.png)

#### **IV. Financial Performance & Resource Cost**
A specialized view of hospital economics. This dashboard correlates clinical activity with revenue, highlighting that senior patients drive the highest lab revenue and pharmacy expenditure.

![Financial Dashboard](dashboard_images/financials.png)

#### **V. Patient Journey (Drill-Through)**
A granular tool for clinical managers. By selecting a specific encounter, users can "drill through" to a detailed patient profile to view medication changes, specific lab results, and diagnostic history.

![Patient Drill Down](dashboard_images/patient_drillthrough.png)



## 5. Key Insights
* **Ethnicity Risk Disparity:** A **12% performance gap** exists between African American patients (35% risk) and the benchmark group (23%).
* **Seasonal Volatility:** Data reveals consistent spikes in **August and October**, suggesting periods of peak system pressure.
* **The "Polypharmacy" Indicator:** Patients prescribed **14+ medications** or undergoing **13+ lab procedures** are significantly more likely to be readmitted.
* **Patient-Level Complexity:** Individual drill-downs reveal that high-risk patients often have lab counts exceeding **150–300 per stay** and frequent medication titration.

> **View More Comprehensive Insights:** [Click here to read the full Business Performance Report (PDF)](./Reports/Hospital_Performance_Report.pdf)



## 6. Strategic Recommendations

* **Implement a "High-Risk Daily List":** Use the **Patient Drill-Through** tool to generate a daily report of patients with >10 medications for a mandatory pharmacist consult prior to discharge.
* **Standardize Discharge Care:** Analyze the protocols used for the 23% risk groups to create a standardized "Gold Standard" discharge checklist for higher-risk segments.
* **Seasonal Staffing Adjustments:** Reallocate nursing and administrative resources to cover the identified surges in August and October to maintain care quality.
* **Specialized Pathways:** Establish a dedicated clinical pathway for **Heart Failure (Code 428)** to reduce the 4.38-day average stay while protecting the revenue baseline.



## **Contributors**

* **Van Tasi:** Lead DAX Developer & Statistical Analysis
* **Selmah Mise:** Senior Dashboard Designer & UI/UX Optimization
* **Peter Kidiga:** Data Engineering (Cleaning & ETL Transformation)
* **Hermela Seltanu:** Data Engineering (Cleaning & ETL Transformation)
* **Hetal Kumbharana:** Data Architect (Star-Schema Modelling) & Dashboard Support




