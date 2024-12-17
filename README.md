# Diabetes 130-US Challenge - Globant
This is a project that was developed as a challenge for applying to the Data Scientist role at Globant. The given dataset is 10-years record of patients with diabetes who where 
at the hospital. So, the input is a set of 49 features related to the patient such as age, gender and race, and others given by the hospital outcomes during the inpatient encounter. 
With that information, the objective is to predict if the patient will be readmitted to the hospital within the firts 30 days of discharge, after the first 30 days, or not readmitted.
To solve the problem, we suggest four main stages: data pre-processing, feature engineering a brief exploratory data analysis and selection and evaluation of the model. 
Below, there is a description of what was done in each stage:

## Data pre-processing:
The dataset was loaded from the repository of UCE Irvini (https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008). The first step looking for unique and 
independent registers, because we found that for a patient ID, it can be found many encounter ID registers. Considering that for ML models it is important to have independent observations,
we keep the first encounter register for each patient. Then, we run an explonatory analysis considering the distribution of the features in terms of NaN registers. With that in mind, 
we decided to exclude the weight feature because its percentage of NaN values (97%). For the medical specialty, we test some alternatives and evaluate if it can be added, despite the high 
number of NaN registers (54%). But the missing values, added up to the fact that this is a categorical variable with more than 50 different values, made us make the decision
to exclude this feature from the analysis too. 
Also, we exclude a list of medications measured during the inpatient encounter (not given, dose increased, dose decreased) because most of the patients do not received those medications
so they would not add information. We delete the patients who died or were given up for dead. Besides, do not include the third diagnosis because it has a high percentage of NaN value 
and because we have information of the diagnosis in diag1 and diag2. 
Finally, the variable "payer code" was exclude because it may not influence the event of the patient being readmitted.
With this data cleaning process, we went from 101,766 to 69,964 observations.

## Feature engineering:
### Input:
The variables discharge_disposition_id, admission_type, and diag1/diag2 were remapped to a group of less categories, which describe them more generally:
* __discharge_disposition_id:__ Home, Home with Health Services, Transferred or Other.
* __admission_type:__ Urgency, Scheduled appointment, Other.
* __diag1/diag2:__ Those where remapped to 10 main categories using as reference the ICD-10 codes.

Besides, we applied the One Hot Encoder to the features 'race', 'gender', 'admission_type_id', 'discharge_disposition_id', 'diag_1' and 'diag_2'.
For the features related to the medications, we transform the possible values into 0 or 1 depending if the medication was given or not. The age was transformed into a numerical variable
using the middle point of the given interval
### Output:

## EDA:

## Model selection and evaluation:

### Logistic regression:

### Decision Tree:

### Random Forest:

## Future work:
NN
