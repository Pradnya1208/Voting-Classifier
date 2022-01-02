<div align="center">
  
[1]: https://github.com/Pradnya1208
[2]: https://www.linkedin.com/in/pradnya-patil-b049161ba/
[3]: https://public.tableau.com/app/profile/pradnya.patil3254#!/
[4]: https://twitter.com/Pradnya1208


[![github](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c292abd3f9cc647a7edc0061193f1523e9c05e1f/icons/git.svg)][1]
[![linkedin](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/9f5c4a255972275ced549ea6e34ef35019166944/icons/iconmonstr-linkedin-5.svg)][2]
[![tableau](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/e257c5d6cf02f13072429935b0828525c601414f/icons/icons8-tableau-software%20(1).svg)][3]
[![twitter](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c9f9c5dc4e24eff0143b3056708d24650cbccdde/icons/iconmonstr-twitter-5.svg)][4]

</div>

# <div align="center">Titanic Voting Classifier</div>
<div align="center"> 
<img src = "https://github.com/Pradnya1208/Voting-Classifier/blob/main/header-titanic-solar-storms-2048x984.jpg?raw=true" >
</div>
  

## Objective:
To predict survival on the Titanic and get familiar with ML basics.<br><br>
The sinking of the RMS Titanic is one of the most infamous shipwrecks in history. On April 15, 1912, during her maiden voyage, the Titanic sank after colliding with an iceberg, killing 1502 out of 2224 passengers and crew. This sensational tragedy shocked the international community and led to better safety regulations for ships.

One of the reasons that the shipwreck led to such loss of life was that there were not enough lifeboats for the passengers and crew. Although there was some element of luck involved in surviving the sinking, some groups of people were more likely to survive than others, such as women, children, and the upper-class.

In this problem, we have done the complete the analysis of what sorts of people were likely to survive. In particular, we applied the tools of machine learning to predict which passengers survived the tragedy.
## Dataset:
[Titanic - Machine Learning from Disaster](https://www.kaggle.com/c/titanic/data)

#### Data Fields:
| Variable              | Definition                                                                |
| ----------------- | ------------------------------------------------------------------ |
| Survival |  Survival|
| pclass |  Ticket Class|
| sex |  Sex|
| Age |  Age in years|
| sibsp |  number of siblings/spouses aboard the Titanic|
| parch |  number of parents/children aboardthe Titanic|
| ticket | Ticket number|
| fare |  Passenger fare|
| cabin |  Cabin Number|
| embarked |  Port of Embarkation|

#### Variable Notes:
- **pclass**: A proxy for socio-economic status (SES)
1st = Upper
2nd = Middle
3rd = Lower

- **age**: Age is fractional if less than 1. If the age is estimated, is it in the form of xx.5

- **sibsp**: The dataset defines family relations in this way...
- **Sibling** = brother, sister, stepbrother, stepsister
- **Spouse** = husband, wife (mistresses and fiancés were ignored)

- **parch**: The dataset defines family relations in this way...
- **Parent** = mother, father
- **Child** = daughter, son, stepdaughter, stepson
Some children travelled only with a nanny, therefore parch=0 for them.


## Implementation:

**Libraries:** `sklearn` `Matplotlib` `pandas` `seaborn` `NumPy` 
## Understanding the data:
#### 1. Sibsp and Parch:
- Small numbers of SibSp have higher probability to survive.
- Small numbers of Parch have higher probability to survive.
![sibsp_parch](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/sibsp_parch.PNG?raw=true)
- Since these features have similar behavior, then we can add them with each person being analyzed.
![family](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/family.PNG?raw=true)
- There are 11 different categories for Family, lets group them in single, medium and big families.
![fam](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/category_fam.PNG?raw=true)

#### 2. pclass:
![pclass](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/pclass.PNG?raw=true)
- This shows that first class has higher probability to survive, probably because of influence.

#### 3. Fare:
-As shown in following figure, the data for Fare has a positive skewness. 
![fare](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/fare.PNG?raw=true)

- As shown in following graph, probably the people who payed more have higher probability to suvive, but there aren't many.
![fare2](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/fare2.PNG?raw=true)

- We get following results after categorizing the Fare:
![fare3](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/fare3.PNG?raw=true)
Here we can see that high fare people have higher probability to survive, and very low fare people have not.

#### 4. Embarked:
![embarked](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/embarked.PNG?raw=true)

#### 5. Ticket:
![ticket](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/ticket.PNG?raw=true)

Checkout the [Notebook](https://github.com/Pradnya1208/Voting-Classifier/blob/main/Voting%20Classifier.ipynb) for complete data analysis and Feature Engineering.
## Model Training and Evaluation:
### Feature Importance:
```
rf = RandomForestClassifier() 
rf.fit(x_train, y_train)
```

![importance](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/features_imp.PNG?raw=true)

### Results of Model training:
| Model              | Accuracy                                                                |
| ----------------- | ------------------------------------------------------------------ |
| Adaboost |  0.8159|
| Extra trees |  0.8350|
| Random Forest |  0.8339|
| Gradient Boost |  0.8361|
| SVC |  0.8170|
| XGBoost |  0.8305|


### Voting Classifier:
```
voting = VotingClassifier(estimators=[('rfc', rf_best), 
                                      ('extc', ext_best),
                                      ('svc', svm_best),
                                      ('gbc',gbm_best),
                                      ('xgbc',xgb_best),
                                      ('ada',ada_best)])

v_param_grid = {'voting':['soft',
                          'hard']} # tuning voting parameter

gsV = GridSearchCV(voting, 
                   param_grid = v_param_grid, cv = 10, 
                   scoring = "accuracy",
                   n_jobs = 6, 
                   verbose = 1)

gsV.fit(X_train,y_train)

v_best = gsV.best_estimator_

gsV.best_score_
```
```
Accuracy: 0.8271
```

#### Confusion Matrix:
![cm](https://github.com/Pradnya1208/Voting-Classifier/blob/main/output/cm.PNG?raw=true)



### Lessons Learned
`Data Imputation`
`Classification algorithms`
`Hyperparameter Tuning`
`Voting Classifier`



## Reference:
- [Voting classifiers](https://www.kaggle.com/avelinocaio/top-5-voting-classifier-in-python/notebook)
### Feedback

If you have any feedback, please reach out at pradnyapatil671@gmail.com


### 🚀 About Me
#### Hi, I'm Pradnya! 👋
I am an AI Enthusiast and  Data science & ML practitioner


[1]: https://github.com/Pradnya1208
[2]: https://www.linkedin.com/in/pradnya-patil-b049161ba/
[3]: https://public.tableau.com/app/profile/pradnya.patil3254#!/
[4]: https://twitter.com/Pradnya1208


[![github](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c292abd3f9cc647a7edc0061193f1523e9c05e1f/icons/git.svg)][1]
[![linkedin](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/9f5c4a255972275ced549ea6e34ef35019166944/icons/iconmonstr-linkedin-5.svg)][2]
[![tableau](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/e257c5d6cf02f13072429935b0828525c601414f/icons/icons8-tableau-software%20(1).svg)][3]
[![twitter](https://raw.githubusercontent.com/Pradnya1208/Telecom-Customer-Churn-prediction/c9f9c5dc4e24eff0143b3056708d24650cbccdde/icons/iconmonstr-twitter-5.svg)][4]


