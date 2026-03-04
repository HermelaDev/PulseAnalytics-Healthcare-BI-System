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



## 2. Strategic Business Questions

### 1. Readmission Risk Analysis  
What is the 30-day readmission rate across different age groups and ethnicities, and where do the most significant disparities occur?

### 2. Clinical Intensity Impact  
Is there a measurable relationship between the number of laboratory procedures and medications administered during initial admission and the likelihood of readmission?

### 3. Operational Performance Evaluation  
Which medical specialties (e.g., Cardiology vs. Internal Medicine) exhibit the highest Average Length of Stay (ALOS), and how does this influence bed occupancy?

### 4. Financial Performance Insight  
How does cost-per-treatment vary across payer categories (insurance vs. self-pay), and what trends are observable over time?

### 5. Discharge Risk Profiling  
Which discharge dispositions (Home, Skilled Nursing Facility, Rehabilitation Center, etc.) are associated with the highest probability of 30-day readmission?



## 3. Data Source & Justification

### Data Source
- UCI Machine Learning Repository
- Kaggle

### Dataset
**Diabetes 130-US Hospitals Dataset (1999–2008)**

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



Contributors:

- Van Tasi

- Selmah Mise

- Peter Kidiga

- Hermela Seltanu

- Hetal Kumbharana
