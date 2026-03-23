# patient_readmission_prediction
This project builds a full analytics pipeline from raw data exploration to a machine learning model, using real world health data. It demonstrates end-to-end analytical skills for health systems, insurance, pharma, and healthtech. (Note: Healthcare is one of the most impactful analyses because it directly affects real people's lives)
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
- Python(Pandas, numpy, maplotlib, seaborn, scikit-learn,XGBo0st)
- GitHub
## Key Findings
- Inpatient visits strongest predictor (263% increase in risk)
- Young adults 20-30 surprisingly highest readmission rate
- Circulatory disease drives most actual readmissions
- Medications 16+ significant risk threshold
- Comorbidity score of 2 shows highest readmission rate (11.39%) (average score was 1.77)
# Notebook 1
## Analysis
### Demographic analysis - How Age, Gender and Race relate to Readmission rates? Identifies WHO IS AT RISK?
- **_AGE Insights_** : According to age analysis, patients aged 20-30 have the highest 30-day readmission rate at 14.24%, higher than the elderly patients. Possible reasons could be dietary habits, less post discharge monitoring compared to elderly patients.
- **_GENDER Insights_** :Although females are higher in count, both the genders show nearly identicl readmission rates.(Females: 11.25%, Males: 11.06%). The difference is negligibe suggesting gender alone is not a strong predictor of 30 day readmission.
- **_RACE Insights_** :Caucasian patients dominate the dataset at 76,099(75% of all patients), nearly 6 times more than any other racial group. Readmission Rates clustered into 2 groups, Caucasian(11.29%) and African American(11.22%) with higher risk, Hispanic(10.41%) and Asian(10.14%) with lower risk._(Although rate differences are small, 1% reduction in Caucasian race patients readmission may impact thousands of patients.)_The rate of Unknown race(8.23%) should be treated with caution due to incomplete data.
### Clinical analysis - How length of stay, medications, lab procedures, and prior visits relate to readmission rates? Identifies WHY THEY ARE AT RISK?
- **_Length of Stay Insights (Monotonic - Strong)_** : Although patients staying 1-4 days represent ~75% of all encounters, readmisison rates steadily increased with lenth of stay. Patients staying 8-10 days show the highest readmission rates(13.72% - 14.35%) suggesting could be the most complex cases.
- **_Number of Medications Insights (Monotonic - Very Strong)_** : A clear monotonic relationship exists between number of medications and 30-day readmission rate. Crossing 10 medications represents a significant risk threshold with a 1.85% jump in readmission rate. This consistent pattern makes num_medications a strong  candidate predictor variable for our ML model.
- **_Number of Lab Procedures (Monotonic - Moderate)_** : A clear monotonic relationship exists between number of lab procedures and 30-day readmission rate. Crossing 31 lab procedures represents a key risk threshold with a 1.803% jump in readmission rate. Compared to medications, lab procedures show a weaker but consistent signal.(Quartile based binning produced balanced groups of ~25000 patients each)
- **_Prior Visits Analysis_** : The **"Inpatient"** the STRONGEST predictor with a 263% increase from None(8.44%) to High(30.70%), Patients with 4+ inpatient visits are 3.6x more likely to be readmitted. **"Emergency"** also exhibits monotonic relationsip confirming strong predictive power. A 173% increase from None(10.47%) to High(28.54%). **"Outpatient"** is a weak predictor as regular outpatient attendance may indicate active health management.
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
# Notebook 2
## Feature Engineering
- _**Age Encoding**_ Converted age brackets to numeric midpoint(preserves the true numeric meaning of age [20-30] is 25) values so the ML model can understand age as a continuous number.
- _**Diagnosis Grouping**_ Converted Converted raw ICD-9 diagnosis codes into 9 meaningful clinical categories across all 3 diagnosis columns.
- _**Comorbidity Score**_ Calculated to understand its relation with Readmission rate. The score signifies the total medical condiions the patient is daignosed with. Score 2 shows highest readmission rate (11.39%) while 
score 3 slightly drops (10.94%) — suggesting highly complex patients receive more careful discharge planning.
- _**Prior Visits Score:**_ This is a weighted score derived from all 3 patient visits. The initial formula showed a max of 160 and 75th perentile at 3. The 99th percentile for emergency visits was 3 indicating extreme outliers ranging upto 76. The emergency visits were capped at 99th percentile to reduce influence of outliers and the score was recalculated. The new max score after capping was reduced to 29.
- _**Medication Changes Flags:**_ : Performed Binary encoding. Medication chnages signal unstable condition, Insulin chnage had readmission rate of 13.46%(2.99% increase)  and other medications was 13.06%(2.61% increase). Bith can be used significantly to predict readmission.
- _**Categorical Encoding**_ Converted remaining text columns to numbers using two strategies:
    - Label Encoding (Binary): Applied to columns with only 2 categories — gender (Male=0, Female=1), change (No=0, Ch=1), diabetesMed (No=0, Yes=1)
    - One Hot Encoding: Applied to nominal columns with 3+ categories and no natural order — race (6 categories), diag_1_group, diag_2_group, diag_3_group (9 categories each).Each category became its own 0/1 column.
# Notebook 3
## Machine Learning Model
- The dataset has a **class imbalance**, only 11.2% of patients were readmitted; Used class_weight='balanced' to force the model to pay equal attention to both classes.
- The data was split 80/20 into training and test sets. Statification implemented for good fit 
### Models Built:
    - Logistic Regression  Baseline model: ROC-AUC: 0.649 — above random baseline (0.5), Recall for readmissions: 53% — finds half of actual cases, Precision for readmissions: 17% — high false alarm rate
    - Random Forest  More powerful model: ROC-AUC: 0.654 — above random baseline (0.5), Recall for readmissions: 1% — predicts all as not readmitted, Precision for readmissions: 59% — high false alarm rate| Accuracy-89% misleading
    - XGBoost  Best performing model: ROC-AUC: 0.617 — below baseline. Recall: 31% — better than Random Forest but still weak. Precision: 18%. Default parameters insufficient for this imbalanced healthcare dataset.
    - Fine Tuned XGBoost: Best Model:** ROC-AUC: 0.684 — highest across all models. Recall: 58% — finds majority of actual readmissions. Precision: 18% — acceptable given high cost of missed readmissions. Key tuning: max_depth=4, learning_rate=0.1, subsample=0.8, colsample_bytree=0.8,  scale_pos_weight=7.96.
    - SHAP Analysis  Model explainability: Top predictors confirmed — discharge_disposition_id (0.25), number_inpatient (0.19), prior_visit_score (0.15), num_medications (0.08). EDA and SHAP findings in strong agreement — high confidence in analytical conclusions.
 - Note:High accuracy ≠ good model for imbalanced data.
# Addl Information
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

## Domain Terms
- ICD-9- International Classification of Diseases 9th Revision 

