# PulseAnalytics: A Strategic BI Framework for Hospital Readmission & Clinical Resource Optimization

## Table of Contents
1. [Problem Statement](#1-problem-statement)
2. [Project Directory](#2-project-directory)
3. [Strategic Business Questions](#3-strategic-business-questions)
4. [Data Source & Characteristics](#4-data-source--characteristics)
5. [Technical Workflow](#5-technical-workflow)
    * [Data Cleaning & Transformation](#data-cleaning--transformation)
    * [Data Modeling (Star Schema)](#data-modeling-star-schema)
    * [DAX Calculations & Measures](#dax-calculations--measures)
6. [Dashboard Pages Overview](#6-dashboard-pages-overview)
7. [Key Insights](#7-key-insights)
8. [Strategic Recommendations](#8-strategic-recommendations)
9. [Project Resources](#9-project-resources)
10. [Contributors](#10-contributors)

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
| `Data/` | Source dataset (`diabetic_data.csv`). |
| `Documentation/` | Technical guides: `Documented_transformations.pdf` and `Documented_modelling.pdf`. |
| `Reports/` | Final business deliverables: `Hospital_Performance_Report.pdf`. |
| `PulseAnalytics.pbix` | Core Power BI file (Data Model & Visualizations). |

---

## 3. Strategic Business Questions
1. **Readmission Risk & Equity Gap:** What is the baseline 30-day readmission rate, and what is the "gap" between our highest-risk (35%) and best-performing (23%) segments?
2. **Seasonal Volume & Timing:** Are there specific months where clinical pressure spikes, necessitating resource reallocation?
3. **Clinical Intensity & Resource Load:** Is there a correlation between high-intensity interventions (medications/labs) and the likelihood of returning within 30 days?
4. **Operational Consistency:** Which primary diagnoses (e.g., Heart Failure - 428) consistently drive hospital volume and ALOS?
5. **Financial Impact:** How does the cost of care scale within our largest patient demographic, the senior population?

---

## 4. Data Source & Characteristics
* **Source:** [UCI Machine Learning Repository - Diabetes 130-US Hospitals](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)
* **Dataset Characteristics:**
    * 101,766 patient admission records.
    * 50+ clinical and administrative variables.
    * Numerical measures: Lab Procedures, Medication Count, Length of Stay.
    * Categorical attributes: Race, Gender, Specialty, Payer Code.

---

## 5. Technical Workflow

### Data Cleaning & Transformation
*Detailed documentation regarding data cleaning steps can be found in:* `Documentation/Documented_transformations.pdf`
* **Handled Missing Values:** Addressed nulls in 'Weight', 'Payer Code', and 'Medical Specialty'.
* **Data Standardization:** Re-grouped Age brackets and mapped ICD-9 codes to clinical categories.

### Data Modeling (Star Schema)
*Detailed documentation regarding the model architecture can be found in:* `Documentation/Documented_modelling.pdf`
* **Fact Table:** `Fact_Admissions` (Numeric measures and keys).
* **Dimension Tables:** `Dim_Patients`, `Dim_Specialties`, `Dim_Payer`, `Dim_Discharge`, and `Dim_Date`.

### DAX Calculations & Measures
*Key measures developed for this analysis include:*
* **Readmission Rate %:** Calculation of 30-day return frequency.
* **Risk Gap:** Percentage difference between demographic risk segments.
* **Resource Intensity Index:** Correlating lab procedures and medication titration.

---

## 6. Dashboard Pages Overview

#### **I. Executive Health Overview**
Tracks the baseline 33% readmission risk and total patient encounters.
![Executive Dashboard](dashboard_images/executive_summary.png)

#### **II. Demographic & Risk Analysis**
Identifies health disparities across ethnicity, age, and gender.
![Risk & Gap Analysis](dashboard_images/readmission_analysis.png)

#### **III. Clinical & Operational Efficiency**
Maps Average Length of Stay (ALOS) against primary diagnoses.
![Operational Dashboard](dashboard_images/operations.png)

#### **IV. Financial Performance & Resource Cost**
Correlates clinical activity with revenue, highlighting senior patient expenditure.
![Financial Dashboard](dashboard_images/financials.png)

#### **V. Patient Journey (Drill-Through)**
Granular tool for clinical managers to view individual medication changes and lab results.
![Patient Drill Down](dashboard_images/patient_drillthrough.png)

---

## 7. Key Insights
* **Ethnicity Risk Disparity:** A **12% performance gap** exists between African American patients (35% risk) and the benchmark group (23%).
* **Seasonal Volatility:** Data reveals consistent spikes in **August and October**, suggesting peak system pressure.
* **The "Polypharmacy" Indicator:** Patients prescribed **14+ medications** are significantly more likely to be readmitted.
* **Patient Complexity:** High-risk profiles often show lab counts exceeding **150–300 per stay**.

> **View Comprehensive Analysis:** [Click here to read the full Business Performance Report (PDF)](./Reports/Hospital_Performance_Report.pdf)

---

## 8. Strategic Recommendations
* **High-Risk Daily List:** Use the Drill-Through tool to flag patients with >10 medications for pharmacist consult.
* **Standardize Discharge:** Model protocols after the 23% risk group to close the 12% equity gap.
* **Seasonal Staffing:** Increase administrative support during identified surges in August/October.
* **Specialized Pathways:** Establish a dedicated pathway for Heart Failure (Code 428) to optimize length of stay.

---

## 9. Project Resources
* **Interactive Dashboard:** [Click Here to View Power BI Public Link](#)
* **Final Presentation Slides:** [Available in Reports Folder](./Reports/Presentation_Slides.pdf)

---

## 10. Contributors

| Contributor | Primary Role | Key Responsibilities |
| :--- | :--- | :--- |
| **Van Tasi** | Lead DAX Developer | Statistical Analysis, Measure Creation, Readmission Logic |
| **Selmah Mise** | Senior Dashboard Designer | UI/UX Optimization, Visual Storytelling, Dashboard Design |
| **Peter Kidiga** | Data Engineer | ETL Transformation, Data Cleaning, Power Query |
| **Hermela Seltanu** | Data Engineer | ETL Transformation, Data Cleaning, Data Profiling |
| **Hetal Kumbharana** | Data Architect | Star-Schema Modelling, Relationship Management, Dashboard Support |