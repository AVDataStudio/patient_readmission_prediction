# patient_readmission_prediction
This project builds a full analytics pipeline from raw data exploration to a machine learning model, using real world health data. It is a learning experience to demonstrate end-to-end analytical thinking helpful for analyst roles in health systems, insurance, pharma, and healthtech. (Note: Healthcare is one of the most impactful analyses because it directly affects real people's lives)
## Why prediction is needed?
Hospital readmissions within 30 days cost the U.S. healthcare system over $26 billion annually. Predicting which patients are at high risk of readmission allows hospitals to intervene early, improve patient outcomes, and reduce costs.
## This project answers
1. What patient demographics and clinical factors are most associated with 30-day readmission?
2. Can this reliably predict which patienst will be readmitted before discharge?
3. Which interventions(by daignosis) would have the highest cost saving impact?
4. How do readmission rates vary across hospital departments and time?
## Datset Source
https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008
## Tools
- Anaconda
- Jupyter
- Python(Pandas, numpy, maplotlib, seaborn)
- GitHub
## Key Findings
- Inpatient visits strongest predictor (263% increase in risk)
- Young adults 20-30 surprisingly highest readmission rate
- Circulatory disease drives most actual readmissions
- Medications 16+ significant risk threshold
## Project Date
03-11-2026
## Dataset Charecteristics 
- Subject area - Health and Medicine
- Feature Type - categorical, Integer
- Instances(rows) - 101766
- Features(columns) - 50
## Data Profiling
- 37 Object(text) Columns
- 13 Integer Columns
- Target Variable: Readmission Categories
## Data Cleaning
- Dropped 5 huge missing columns(missing % above 40)
- Filled 4 low missing columns
## Feature Engineering
- Binary Target Variable: Converted text categories into number, also called as label encoding.
- Medications Bins: Converted raw medication counts into meaningful groups.
## Analysis
### Demographic analysis - How Age, Gender and Race relate to Readmission rates? Identifies WHO IS AT RISK?
- **_AGE Insights_** : According to age analysis, patients aged 20-30 have the highest 30-day readmission rate at 14.24%, higher than the lederly patients. Possible reasins could be dietary habits, Less post discharge monitoring compared to elderly patients.
- **_GENDER Insights_** :Although Females are higher in count, both the genders show nearly identicl readmission rates.(Females: 11.25%, Males: 11.06%). The difference is negligibe suggesting gender alone is not a strong predictor of 30 day readmission.
- **_RACE Insights_** :Caucasian patients dominate the dataset at 76,099(75% of all patients), nearly 6 times more than any other racial group. Readmission Rates clustered into 2 groups, Caucasian(11.29%) and African American(11.22%) with higher risk, Hispanic(10.41%) and Asian(10.14%) with lower risk._(Although rate differences are small, 1% reduction in Caucasian race patients readmission may impact thousands of patients.)_The rate of Unknown race(8.23%) should be treated with caution due to incomplete data.
### Clinical analysis - How length of stay, medications, lab procedures, and prior visits relate to readmission rates? Identifies WHY THEY ARE AT RISK?
- **_Length of Stay Insights (Monotonic - Strong)_** : Although patients staying 1-4 days represent ~75% od all encounters, readmisison rates steadily increased with lenth of stay. Patients staying 8-10 days show the highest readmission rates(13.72% - 14.35%) suggesting could be the most complex cases.
- **_Number of Medications Insights (Monotonic - Very Strong)_** : A clear monotonic relationship exists between number of medications and 30-day readmission rate. Crossing 10 medications represents a significant risk threshold with a 1.85% jump in readmission rate. This consistent pattern makes num_medications a strong  candidate predictor variable for our ML model.
- **_Number of Lab Procedures (Monotonic - Moderate)_** : A clear monotonic relationship exists between number of lab procedures and 30-day readmission rate. Crossing 31 lab procedures represents a key risk threshold with a 1.803% jump in readmission rate. Compared to medications, lab procedures show a weaker but consistent signal.(Quarile based binning produced balanced groups of ~25000 patients each)
- **_Prior Visits Analysis_** : The **"Inpatient"** the STRONGEST predictor with a 263% increase from None(8.44%) to High(30.70%), Patients with 4+ inpatient visits are 3.6x more likely to be readmitted. **"Emergency"** also exhibits monotonic realtionsip confirming strong predictive power. A 173% increase from None(10.47%) to High(28.54%). **"Outpatient"** is a weak predictor as regular outpatient attendance may indicate active health management.
- **_Diagnosis Group Analysis_** : Diabetes has the highest readmission rate at 12.98% but circulatory disease has the highest absolute readmission volume due to its large patient count (30,437 patients). **_Relative impact(rate) vs Absolute impact(count*rate)_**:The distinction between relative rate and absolute impact is critical for hospital resource planning.
### Our complete predictor summary so far:
    |Feature           | Pattern       | Strength     |
    | Lebgth of Stay   | Monotonic     | Strong       |
    | Medications      | Monotonic     | Very Strong  |
    | Lab Procedures   | Monotonic     | Moderate     |
    | Inpatient visits | Monotonic     | Strongest    |
    | Emergency visits | Monotonic     | Strong       |
    | Outpatient visits| Flat          | Weak         |
    | Diagnosis group  | Varies by Cond| Strong       |
### Correlation Analysis 
- Pearson correlation only captures LINEAR relationships. Several features like number_inpatient show non-linear exponential patterns that correlation underestimates.
## Business Recommendations
- Hospital should implement targeted follow up programs for younger age groups, not just elderly.
- Hospital readmission reduction programs should prioritize Caucasian and AfricanAmerican patients given  their combination of high volume and elevated readmission rates. AN unusual drop to 10.51% observed in case of 11 days, reflecting either a careful discharge planning or a small sample size(n=1,855).
- Patients with 7+ days stay should be flagged for enhanced discharge planning and post discharge follow up calls to reduce readmission risk.
- Patients prescribed 16+ medications should be automatically flagged for medication reconciliation review before discharge to reduce readmission risk.
- Patients with 32+ lab procedures  should be considered for enhanced post discharge monitoring as they represent elevated readmission risk.
- Prior inpatient visit count should be the PRIMARY flag for high risk patient identification. Any patient with 2+ prior inpatient visits should automatically trigger an enhanced discharge planning protocol.
- For targeted clinical programs → focus on Diabetes (highest rate)
- For budget and resource planning → focus on Circulatory disease as it drives the most actual readmissions
 
