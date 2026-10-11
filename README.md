# <p align="center">Hospital Readmission Risk Analysis</p>

## Problem Statement
When a patient returns to the hospital within 30 days of discharge, the hospital can be hit with financial penalties. This project studies a decade of records from 130 U.S. hospitals to understand which patients, diagnoses, and care patterns are most associated with early readmission. The aim is to give care teams a way to spot high-risk patients, direct follow-up effort where it matters most, and reduce readmissions that could have been avoided.

## Dataset & Tech Stack
- **Dataset**: Diabetes 130-US Hospitals (1999-2008), sourced from the UCI Machine Learning Repository under CC BY 4.0. It contains roughly 100,000 patient encounters from 130 hospitals.
- **Database**: MySQL Workbench
- **SQL Concepts Applied**: CTEs, window functions (RANK, NTILE), CASE-based risk tiers, correlated subqueries, and joins across multiple tables

## Methodology
- **Data cleaning**: Converted placeholder values into proper NULLs, dropped a column that was almost entirely empty (~97% missing), retained a single encounter per patient to prevent duplicate-driven bias, and excluded patients who had died or been discharged to hospice.
- **Exploratory analysis**: Set baseline readmission rates across age groups, admission types, and diagnosis categories.
- **Advanced analysis**: Used a CTE to define a reusable high-risk patient cohort, ranked patients by risk with window functions, segmented them by medication use and diagnosis count, and identified diagnosis categories whose readmission rates sit above the overall average.
- **Reporting**: Condensed the results into four main findings, each tied back to the original business problem.

## SQL Techniques Highlighted
- **CTEs** to build a clean, reusable risk cohort that multiple queries can draw on
- **Window functions**: `RANK() OVER (PARTITION BY ...)` to rank medication load within each age group, and `NTILE(4)` to split patients into risk quartiles
- **CASE-based tiering** to turn continuous variables into risk categories that business users can easily interpret
- **Correlated subqueries with HAVING** to isolate diagnosis categories that outperform the population-wide average in readmissions
- **Multi-table joins** with ID-mapping reference tables to replace numeric codes with readable labels

## Key Findings
1. **Circulatory and diabetes-related diagnoses** recorded the **highest readmission rates** of all diagnosis categories, and both were above the overall average.
2. Patients with **a greater number of inpatient visits in the previous year** were at noticeably higher risk of readmission, making past utilization a strong predictor.
3. **Discharge destination had a clear impact.** Readmission rates for patients sent to certain facility types differed measurably from those discharged to their homes.
4. Results on **medication changes at discharge and A1C testing during the stay** are consistent with the clinical question the dataset was originally gathered to explore.

## Recommendations
- Prioritize discharge planning and follow-up for patients with the heaviest history of prior hospital visits.
- Provide additional care coordination for patients with circulatory or diabetes-related conditions, as they are readmitted more often.
- Audit discharge practices at facilities with elevated readmission rates to uncover where the process can be improved.
