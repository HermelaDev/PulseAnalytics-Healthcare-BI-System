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



### B. Data Modeling Strategy (Star-Schema)

Based on the dataset structure and the nature of the hospital encounters, our star schema consists of one central fact table (FACT_ENCOUNTER) containing all numeric mesaures and foreign keys, surrounded by four dimensions: Dim_Patient [patient demographics], Dim_Admission [admission characteristics], Dim_Diagnosis [ICD-9 codes], and Dim_Medication [drug dosage changes]. This modelling approach ensures optimal performance, accurate DAX calcualtions, and clean filter propagation. 

The Star Schema consists of:

#### Fact Table
**Fact_Encounter**
- encounter_id
- patient_nbr
- admission_type_id
- admission_source_id
- discharge_disposition_id
- diag_1
- diag_2
- diag_3
- time_in_hospital
- num_lab_procedures
- num_procedures
- num_medications
- number_outpatient
- number_emergency
- number_inpatient
- number_diagnosis
- readmitted
- Admission_Key
- Diagnosis_Key
- Synthetic_Date

#### Dimension Tables
- **Dim_Patient** (patient_nbr, race, gender, Age_Midpoint, payer_code)
- **Dim_Admission** (admission_type_id, admission_source_id, discharge_disposition_id, Admission_ID, Admission_Key)
- **Dim_Diagnosis** (diag_1, diag_2, diag_3, Diagnosis_ID, Diagnosis_Key)
- **Dim_Medication** (Drug_Name, Dosage_Change_Type, Medication_ID, Medication_Key)
- **Dim_Date** (Dedicated Date table built using DAX)

The model implements:
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
