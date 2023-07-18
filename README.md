# Jay-Kalantar-Practical-Application-Assignment-17.1
Submit the website URL to my public-facing GitHub repository for my third practical assignment.


# Objective:   
### To predict whether a client will subscribe to a term deposit based on the data provided.

# Data Understanding:
### The dataset contained 21 column. 11 Numeric and the rest are categorical:
1. Data is imbalanced with 88.7% of the data being 'no' and 11.3% being 'yes'. 
2. There are null values in the dataset listed as 'unknown'. 
3. The data is categorical and will need to be encoded for modeling.


#### bank client data:
1) age (numeric)
2) job : type of job (categorical: 'admin.','blue-collar','entrepreneur','housemaid','management','retired','self-employed','services','student','technician','unemployed','unknown')
3) marital : marital status (categorical: 'divorced','married','single','unknown'; note: 'divorced' means divorced or widowed)
4) education (categorical: 'basic.4y','basic.6y','basic.9y','high.school','illiterate','professional.course','university.degree','unknown')
5) default: has credit in default? (categorical: 'no','yes','unknown')
6) housing: has housing loan? (categorical: 'no','yes','unknown')
7) loan: has personal loan? (categorical: 'no','yes','unknown')
#### related with the last contact of the current campaign:
8) contact: contact communication type (categorical: 'cellular','telephone')
9) month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')
10) day_of_week: last contact day of the week (categorical: 'mon','tue','wed','thu','fri')
11) duration: last contact duration, in seconds (numeric). Important note: this attribute highly affects the output target (e.g., if duration=0 then y='no'). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.
#### other attributes:
12) campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)
13) pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)
14) previous: number of contacts performed before this campaign and for this client (numeric)
15) poutcome: outcome of the previous marketing campaign (categorical: 'failure','nonexistent','success')
#### social and economic context attributes
16) emp.var.rate: employment variation rate - quarterly indicator (numeric)
17) cons.price.idx: consumer price index - monthly indicator (numeric)
18) cons.conf.idx: consumer confidence index - monthly indicator (numeric)
19) euribor3m: euribor 3 month rate - daily indicator (numeric)
20) nr.employed: number of employees - quarterly indicator (numeric)

#### Output variable (desired target):
21) y - has the client subscribed a term deposit? (binary: 'yes','no')

### The following features are missing values and we need to impute or drop them
	
	job                0.80%
	marital            0.19%
	education          4.20%
	default           20.87%
	housing            2.40%
	loan               2.40%
	

# Data Preparation:
1) Replaced unknown with NaN values
2) One-Hot encode all object features
3) Impute missing values using KNN Imputer
4) Split Train / Test (20%)
5) Scaled Train / Test ('age', 'duration', 'campaign', 'pdays', 'previous','cons.price.idx', 'cons.conf.idx', 'euribor3m', 'nr.employed')



# Summary of the Findings

## Simple Modeling with the first 7 columns:  
| model | Train Time | Train Accuracy |	Test Accuracy |	Precision |	Recall |
| ----- | ---------- | -------------  | -----------   | -----------   | -----------   |
|KNN|241 ms|0.756|0.679|0.679|0.679|
|Logistic Regression|151 ms	|0.754|0.768|0.768|0.768|
|Decision Tree|	124 ms|0.853|0.560|0.566|0.560|
|SVM|129 ms|0.756|0.767|0.655|0.767|


## Model Impovements:
1) Use the full feature set 
2) Compare the performance of the classifiers: K Nearest Neighbor, Logistic Regression, Decision Trees, and Support Vector Machines
3) Tune the hyperparameters of the best performing classifier


| Model | Train Score |	Test Score |	Best Score | Accuracy | Precision |	Recall |	F1-score |	Avg fit time (sec) |	Fits | Best Hyper Parameters |
| ----- | ---------- | -------------  | -----------   | -----------   | -----------   | -------------  | -----------   | -----------   | -----------   | -----------   |
Base|0.889|0.881|||||			
Logistic Reg.|0.912|0.910|0.911|0.91|0.9|0.91|0.9|1.239|280|C=1, max_iter=1000, penalty=l2
Decision Tree|0.916|0.915|0.913|0.92|0.91|0.92|0.91|0.713|810|criterion=gini, max_depth=5, min_samples_split=2
KNN|1.000|0.904|0.908|0.9|0.89|0.9|0.89|0.133|270|metric=euclidean, n_neighbors=17, weights=dist...
SVC|0.921|0.908|0.91|0.91|0.9|0.91|0.9|101|20|kernel=rbf



## Next steps and recommendations:

1) The data is highly imbalance, so using precision and recall is the better measure of the model performance.  I used F1-Score which is the mean of precision and recall
2) The seven feature model resulted in much lower precsion and recall than utilizing all features model (precision 0.76 vs 0.91)
3) Both Logistic Regression and Decision Tree provided the best fits and good performance in both seven and full feature models
4) The Feature Importance from the Logistic Regression better fit the manual feature examinations
5) Both KNN and SVM performed much slower without any imporvement on the model performance
6) Both KNN and SVM do NOTnot provide feature importance readily making them less useful for drawing inferences
7) I tried 4 different types of imputing (including KNN) they all had very little to no effect since the Nan values are only 7% of the total and don't really drive anything.
8) I also tried removing some features again, no impact on the final outcome.
9) Perhaps, we can segment the data by education or job to identify customers with higher offer acceptance.