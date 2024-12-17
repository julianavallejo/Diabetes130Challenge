# Diabetes 130-US Challenge - Globant
This is a project that was developed as a challenge for applying to the Data Scientist role at Globant. The given dataset is 10-years record of patients with diabetes who where 
at the hospital. So, the input is a set of 49 features related to the patient such as age, gender and race, and others given by the hospital outcomes during the inpatient encounter. 
With that information, the objective is to predict if the patient will be readmitted to the hospital within the firts 30 days of discharge, after the first 30 days, or not readmitted.
To solve the problem, we suggest a classification approach, and propose four main stages: data pre-processing, feature engineering a brief exploratory data analysis and selection and evaluation of the model. 
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
The features related to the medications, change and diabetesMed were transformed into boolean vectors (0, 1). The age was transformed into a numerical variable
using the middle point of the given interval. Finally, A1Cresult was Label Encoded, and the rest of numerical variables were scaled using MinMax Scaler. 
### Output:
As it was mentioned, the original output has 3 categories: readmitted before 30 days after discharge, readmitted 30 days later, or not readmitted. For this project, it was transformed into 2 main categories: readmitted or not readmitted. 
This was done considering the scope of the project and also, because this transformation significantly reduces the challenge of classes imbalance in this classification problem.
## EDA:
With the processed data set, we run an exploratory data analysis (EDA) and implemented Principal Component Analysis (PCA) but encountered unexpected results. 
Specifically, we found that a single feature explained more than 90% of the variance. When we trained the model using the first five principal components, the performance metrics were significantly poor. 
We suspect this behavior is due to the features being mostly binary, resulting in a sparse X matrix that may have negatively affect the PCA calculations. 
Given the time constraints and the scope of our current problem, we decided to stop the EDA at this point. 
However, for future work, it would be interesting to go deeper into this issue to better understand the underlying causes, reduce the dimensionality of the problem, and potentially improve the model's performance.

## Model selection and evaluation:
As this is a classification problem, we tested three of the most used models in this context. From the simplest to the more complex, we tested logistic regression, decision trees, and random forest. The next sections describe some considerations of the results found.
### Logistic regression:
The selected solver is liblinear because its speed and good performance in large-scale linear classification problems. Besides, it can handle sparse input matrix which is our case. 
Logistic regression shows consistent performance between training and validation sets. However, it has a low recall, indicating it misses a significant number of true positives. 
The precision is moderate, suggesting it is relatively good at predicting true positives when it does make a positive prediction.

### Decision Trees:
As this model is prone to overfitting, we tested 5 different max_depth and keep the one that shows good accuracy in the training dataset. The decision tree has slightly better recall and F1 score compared to logistic regression, but its AUC and accuracy are similar. 
The performance drops a bit from training to validation, indicating potential overfitting.

### Random Forest:
For this model, we tested different numbers of estimartors (trees) and check the accuracy. We keep them in 100 as it is a suggested number. Random forest has the highest AUC, indicating it is the best at distinguishing between classes. However, its recall is extremely low, meaning it fails to identify most of the true positives. 
The high precision suggests that when it does predict a positive, it is usually correct, but the low recall makes it less useful for this problem.
Given the goal is to predict whether a diabetes patient will be readmitted, recall is a crucial metric because missing a readmission could have significant consequences.


## Conclusions and future work:
The decision tree might be the best choice here due to its better balance between recall and precision, despite the slight overfitting. 
However, it might also be considered tuning the hyperparameters of the random forest to improve its recall, as it has the highest AUC and precision.
