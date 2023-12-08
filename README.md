# fairness_metrics_package
## Description:
This package is used to determine the fairness of an AutoML classification or regression model. It can also be used to determine the fairness of self-made models. 
## Features
Within the package there are 10 fairness metrics under 3 categories:
| | | Metric/Function Name | Function |
| ------------------------- | ------------------------- | ------------------------- | ------------------------- |
| <td rowspan="3">-</td> | All Metrics | run_all(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | All Classification Metrics | run_all_classification(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | All Regression Metrics | run_all_regression(_dataset_, _predictedColumn_, _predictedColumn_, _group_) |
| <td rowspan="2">Parity-based Metrics</td> | Statistical/Demographic Parity | stat_demo_parity(_dataset_, _predictedColumn_, _group_) |
|  | Disparte Impact | disparte_impact(_dataset_, _actualColumn_, _group_) |
| <td rowspan="6">Confusion Matrix-based Metrics</td> | Equal Opportunity | equal_opportunity(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | Equalized Odds | equalized_odds(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | Overall Accuracy Equality | overall_accuracy_equality(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | Conditional Use Accuracy Equality | conditional_use_accuracy_equality(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | Treatment Equality | treatment_equality(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | Equalizing Disincentives | equalizing_disincentives(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
| <td rowspan="2">Score-based Metrics</td> | Differences in Squared Error | differences_in_squared_error(_dataset_, _predictedColumn_, _actualColumn_, _group_) |
|  | Balance between Subgroups | balance_btwn_subgroups(_dataset_, _predictedColumn_, _actualColumn_, _group_) |

## About Each Metric
### Statistical/Demographic Parity
This parity will take a look at the number of predicted passed values and divide it by the total number of values for that group. This is one of the easiest to pass and it could be due to the fact that it is just looking at the broad overview of what is predicted to pass, not necessarily if those predictions are correct or not so the accuracy of the model doesn’t really affect the result. Another thing to consider is since the groups are being compared to each other, technically if the model predicted 100% to pass for both or 0% to pass for both whether these predictions are right or wrong it doesn’t matter, it would still pass which I mean technically speaking it is fair to each group, but that’s not really what you want in a model.

### Disparate Impact
This one looks at the same ratio of predicted passing values over total values as statistical demographic parity but instead divides the smallest ratio by the largest and checks to see if its over 80%.

### Equal Opportunity
For this one we will only be looking at the true positives and the false negatives aka the values that have actually passed which is also known as the True Positive Rate. All equal opportunity does is look and compare the true positive rates between each of the groups. Because equalized odds only looks at the true positive rate, there could be unknown bias on the side of false positives that wont even get accounted for.

### Equalized Odds
This metric takes a look at both True Postive Rate and False Positive Rate and is a variation of Equal Opporunity. This is a step further than just TPR in equal opporunity and somewhat helps the issue above. Since it is similar to Conditional Use Accuracy Equality, you may want to use this instead: For false positives, it may be important to look at in contexts where the model prediction has high-stake consequences like if a false positive predicts a man guilty and he get sent to jail for the next 50 years. This is where you want to minimize the differences in the false positive rate between each group.


### Conditional Use Accuracy Equality
This is another variation of Equal Opporunity where it looks at the True Positive Rate and the True Negative Rate. Since it is similar to Equalized Odds, you may want to use this instead: For True negatives, it could be important to look at this if the context of your prediction is something where having true negatives is more important like in medical context of predicting people who may have cancer, you want to prioritize those true negatives because you don’t want to falsely predicted there is no cancer when there is some.

### Overall Accuracy Equality
For this metric we look at the predicted scores that were predicted correctly and divide it by the total number of records. This is very similar to the demographic statistical parity but elevates it to look whether the prediction was correct or not instead of just if they predicted pass. milar to other metrics, it only looks at predicted correctly so the proportion of correct predictions to incorrect predictions is unknown when determining fairness.

### Equalizing Disincentives
For equalizing disincentives, it is a variant of equalized odds so instead of looking at true positive rate and false positive rate separately, we subtract tpr by fpr. The issue with this one is if the fpr is higher than tpr it results in a negative, in these cases the results would just be N/A

### Treatment Equality
For treatment equality, this looks at the ratio between false positive rate and false negative rate. This is very hard to pass because depending on fnr being higher or lower than fpr, it could end up with drastic ratios and it could easily result in a fail. An example I have is if we think about these two ratios:
0.9/0.5 = 1.8 --> *.8 = 1.44
0.4/0.85 = 0.588

### Balance Between Subgroups
For each record its going to find the absolute value of the difference between each predicted and actual value so if predicted is 79 and the actual was 81 the difference would be 2, and it takes that value for each record, adds it up and divides by the total number of records. Its basically just the average difference between all sets of predicted and actual values

### Difference in Squared Error
This is the same as the Balance Between Subgroups except instead of absolute value, the difference between predicted and actual is squared. This makes it harder to pass because if the misprediction is far from truth, when squaring it, the mistakes get punished harder

## Installation
https://pypi.org/project/fairness-metrics/0.0.1/
Run: pip install fairness-metrics==0.0.1


