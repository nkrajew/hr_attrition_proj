# HR Analytics - Employee Attrition: Project Overview
*Disclaimer: Data is fictitious*

- Created an Extreme Gradient Boosting model using SMOTE to predict if an employee would leave the company (Recall: 0.77, Accuracy: 0.88).
- Compared output to XGB model without SMOTE (Recall: 0.31, Accuracy: 0.86).
- Identified Job Role - Healthcare Rep, Job Level 1, Job Involvement 4, and Business Travel - Non-Travel as most important factors.
- Recommended mitigation strategies for each factor (detailed below).

## Code and Resources Used
**Python Version:** 3.7 \
**Packages:** pandas, numpy, matplotlib, seaborn, XGBoost, sklearn, graphviz, imblearn, collections\
**Project Idea:** https://towardsdatascience.com/14-data-science-projects-to-do-during-your-14-day-quarantine-8bd60d1e55e1 (Terence S)
**Model Inspiration:** https://www.kaggle.com/vincentlugat/ibm-attrition-analysis-and-prediction/notebook

## Data Source
Kaggle - https://www.kaggle.com/pavansubhasht/ibm-hr-analytics-attrition-dataset \
The dataset is provided by IBM and is fictitious.

## Summary
The four most important factors in determining if an employee will leave the company or not are as follows:
1. JobRole = Healthcare Representative
2. JobLevel = 1
3. JobInvolvement = 4
4. BusinessTravel = Non-Travel

### Recommendations
**JobRole_Healthcare Representative** - I would recommend that HR investigate the working conditions for this job role. Some possible areas to explore would be how the role compares to others in similar industries, what the leadership is like for these employees, and how the expectations of the role are presented during the hiring process.

**JobLevel_1** - I would recommend exploring ways to make employees feel more connected to the company. This could lead to a larger HR exercise in creating a more collaborative and immersive company culture, creating more job levels to instill a sense of career progression, or creating a clearly defined way to progress within the company (e.g. how healthcare workers have a clinical ladder).

**JobInvolvement_4** - About 10% of employees report a Job Involvement of 4 with the majority of employees reporting a 3. It may be the case that those employees with the most job involvement feel overworked or feel more pressure to deliver. I would recommend the company investigate what factors contribute to an employee reporting high levels of job involvement. This could illuminate negative aspects of their role and the company could try to mitigate them.

**Business Travel - NonTravel** - My hunch is that the location of this node on the decision tree decides its impactfulness (i.e. the importance of non-travel is very path specific within the tree). Until further investigation can be done, I would recommend the company ignore focusing on this feature. More details are given in the Predictive Analysis notebook.

## EDA Findings
- **Income:** Lower earners are more likely to leave the company
- **Work Environement:** Lower environment satisfaction leads to a higher likelihood of attrition. Approximately 1/3 of employees that work OT leave the company.
- **Education:** Education seemingly played no role in an employee's decision to leave the company.
- **Work Experience:** More inexperienced employees are more likely to leave the company. Lesser tenured employees are more likely to leave the company. Employees newer to their roles are more likely to leave the company.
- **Job Performance:** Evaluating job performance was dropped from the analysis after realizing the dataset only contained "Excellent" and "Outstanding" employees

![alt text](https://github.com/nkrajew/hr_attrition_proj/blob/master/inc_attrit.PNG "Income and Attrition")
![alt text](https://github.com/nkrajew/hr_attrition_proj/blob/master/env_sat.PNG "Environment and Attrition")
![alt text](https://github.com/nkrajew/hr_attrition_proj/blob/master/attrit_by_tenure.PNG "Tenure and Attrition")

## Predictive Analysis
### Target Variable
Upon investgation of the target variable, I found I was dealing with an imbalanced dataset.
![alt text](https://github.com/nkrajew/hr_attrition_proj/blob/master/imbalanced.PNG "Imbalanced Target")

### Feature Engineering
I created five new variables that I thought could be helpful in predicting an employees likelihood to leave:
- **LonDis (Long Distance):** An employee comutes 10 miles or more
- **ColDeg (College Degree):** An employee has obtained a college degree
- **LowIncLowJS (Low Income, Low Job Satisfaction):** An employee is being paid less than the median for his role and has job satisfaction of 1
- **Loyalty:** Total working years divided by number companies worked for (higher number means more loyal)
- **CompLoyalty (Loyalty to this Company):** Total years at company divided by total number of working years (best is 1)

### Models and Performance
**1. XGB Model on Imbalanced Dataset**
#### Performance
My primary evaluation was done on Recall and the AUC of the precision-recall curve (reason explained in notebook).
| Recall | Precision | F1   | Accuracy | AUC  |
|--------|-----------|------|----------|------|
| 0.31   | 0.47      | 0.38 | 0.86     | 0.42 |

![alt text](https://github.com/nkrajew/hr_attrition_proj/blob/master/curve_xgb.PNG "XGB Curve")

**2. XGB Model with SMOTE**
#### Brief SMOTE Explanation
Since I was dealing with an imblanced dataset, I needed a way to "balance" out the target distribution. Doing some research I found that SMOTE was a popular and powerful way to do this. Essentially what SMOTE does is oversample the minority target ("Yes" for Attrition) by synthesizing new samples. I paired this together with an undersampling of the majority target to create a new blended dataset with a ratio of 1:2 for Yes-to-No of the target.  

#### Performance
My primary evaluation was done on Recall and the AUC of the precision-recall curve (reason explained in notebook).
| Recall | Precision | F1   | Accuracy | AUC  |
|--------|-----------|------|----------|------|
| 0.77   | 0.85      | 0.81 | 0.88     | 0.89 |

![alt text](https://github.com/nkrajew/hr_attrition_proj/blob/master/curve_smote_xgb.PNG "SMOTE Curve")

### Conclusions
The XGB model using SMOTE performed much better than the model trained on an imblanaced dataset. I used the feature_importances_ property of the XGB model to develop recommendations for the company.

![alt text](https://github.com/nkrajew/hr_attrition_proj/blob/master/xgb_fi_graph.PNG "FI SMOTE")
