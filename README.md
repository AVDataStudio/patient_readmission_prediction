# patient_readmission_prediction
This project builds a full analytics pipeline from raw data exploration to a machine learning model, using real world health data. It is a learning experience to demonstrate end-to-end analytical thinking helpful for analyst roles in health systems, insurance, pharma, and healthtech.
## Why prediction is needed?
Hospital readmissions within 30 days cost the U.S. healthcare system over $26 billion annually. Predicting which patients are at high risk of readmission allows hospitals to intervene early, improve patient outcomes, and reduce costs.
## This project answers
1. What patient demographics and clinical factors are most associated with 30-day readmission?
2. Can this reliably predict which patienst will be readmitted before discharge?
3. Which interventions(by daignosis) would have the highest cost saving impact?
4. How do readmission rates vary across hospital departments and time?
## Datset Source
https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008
For this project the datset was directly imported in python using _**ucimlrepo**_. (more information available through link)
## Tools used
Jupyter Notebook
Python (Pandas, numpy, maplotlib, seaborn, warnings)
GitHub
## Project Date
03-11-2026
## Dataset
Charecteristics - Multivariate
Subject area - Health and Medicine
Feature Type - categorical, Integer
Instances(rows) - 101766
Features(columns) - 50
## Data Profiling
37 Object(text) Columns
13 Integer Columns
Target Variable: Readmission Categories
## Data Cleaning
Dropped 5 huge missing columns(missing % abive 40)
Filled 4 low missing columns
## Analysis
-Demographic analysis - How Age, Gender and Race relate to Readmission rates? This is to identtify high risk groups.
     **_AGE Insights_** : According to age analysis, patients aged 20-30 have the highest 30-day readmission rate at 14.24%, higher than the lederly patients. Possible reasins could be dietary habits, Less post discharge monitoring compared to elderly patients.
      **_GENDER Insights_** :Although Females are higher in count, both the genders show nearly identicl readmission rates.(Females: 11.25%, Males: 11.06%). The difference is negligibe suggesting gender alone is not a strong predictor of 30 day readmission.
## Business Recommendation
- Hospital should implement targeted follow up programs for younger age groups, not just elderly.

 
