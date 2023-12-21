# Analysis Report

## Overview of the Analysis

### Purpose

- The purpose of this analysis is to support a lending companies ability to identify potential borrowers who have either a `healthy` or `high-risk` credit risk.

### Data Information and Prediction

- The financial information dataset contained historical data on previous borrowers.
- For each borrower the dataset contained information on:
  - Loan size.
  - Their income.
  - How much debt they had.
  - Whether the loan had defaulted or not.
- Using this data, I was tasked with creating a model that could predict if future potential borrowers would also default on their loan.

### Variables

- The variables I'm trying to predict are contained within the `loan_status` column.
- This column contains two values:
  - `0` - The loan *has not* defaulted
  - `1` - The load *has* defaulted
- I conducted a `value_counts` check on the `loan_status` column and found:
  - There are 77,536 total records in the dataset
  - Of these records:
    - 75,036 (96.77%) are `0` (healthy) records
    - 2,500 (3.22%) are `1` (high-risk) records
- My initial expectation is that due to the imbalance in dataset records, the first model created will have more difficulty predicting the `1` records over the `0` records

### Machine Learning Data Preparation

- To create the Machine Learning model:
  - Firstly I seperated the dataset into two variables:
    - `X` - This contained all the "features" that will be evaluated from the dataset to determine an outcome on the `y` (target) variable.
    - `y` - The "target" value. This contains all the values for the `loan_status` column for which the model must be trained to predict.
  - With the variables separated:
    - The dataset is split into two groups:
      - `Train` - The data that the machine learning model will be trained on - typically 75% of the dataset.
      - `Test` - The data the machine learning model will be asked to predict based on it's previous learning - typically the remaining 25% of the dataset.

### Logistic Regression / Random Over Sampler

- The machine learning model is a Logistic Regression model.
  - Logistic Regression is a classification machine learning tool which predicts binary classes (either 0 or 1).
  - The model is `fit` (trained) on the split training data and provided the "features" from the `X` variable and the corresponding `y` values.
  - With the model trainied, I can then pass the test data from the `X` variable to predict the outcomes of their `y` values.
  - The predicted `y` values can then be compared to the actual `y` values to determine the accuracy, precision and recall scores of the model.
- There were two models created:
  - The first model was based on the original data provided.
  - The second model makes use of the `RandomOverSampler` module and resamples the data due to to the imbalance of the `y` classes.
    - The resampled data is then passed through a new logistic regression model to determine the effectiveness of resampling the data.

## Results

* Machine Learning Model 1:

  - Balanced Accuracy Score
    - Looking at the result of the balanced accuracy score, we can determine that the model has a 94.4% accuracy, which is calculated through balanced sample weights due to the imbalance of the test dataset.

  - Classification Report
    - Within the classification report we can see the model has a 100% precision, recall and f1-score for the `0` (healthy) label.
    - On the other hand, the report shows that the `1` (high-risk) label only has an 87% precision rate, an 89% recall rate, and a 88% f1-score.
      - This could be down to the imbalance of the dataset. There are only 625 data points for the `1` label, but near 19,000 for the `0` label.
    - This clearly shows the model does a good job of predicting borrowers with `0` (healthy) records, but has less presion and accuracy for predicting the `1` (high-risk) labels, which in the financial lending industry could have a real impact on the success/profitability of the lending company.

* Machine Learning Model 2:

  - Balanced Accuracy Score
    - From using the `RandomOverSampler` module to resample the data, the balanced accuracy score has increased to 99.6%. This in an incrase of 5.2% over the previous models balanced accuracy score using the original data. *(94.4% -> 99.6%)*

  - Classification Report
    - The classification report repeats a 100% precision, recall and f1-score for the `0` (healthy) label.
    - The `1` (high-risk) label continues to have a 87% precision rate, but an increased recall rate of 100% and f1-score of 93%.
    - The classification report shows there is an improvement on the model in terms of predicting the `1` labels.

## Summary

- In my opinion the second model appears to perform best and would be my recommendation as the model to use.
- It's evident that both models do a good job of predicting the `0` labels on both occasions. However, within the lending market the focus has to be on identifying those that fall into the `1` class.
  - This is where problems can arise and the company could end up losing money rather than making it.
- The confusion matrix for the second model shows:
  - Capability to correctly predict 18,668 `True Negatives`, and 623 `True Positives`.
  - The model predicted 91 `False Positives`, which is an increase of 11 (12%) compared to the previous model.
    - This does unfortunatley mean that some borrowers are incorrectly labeled as `1` (high-risk), but does protect the lending company's assets.
  - The main improvement for this model comes from it's number of `False Negatives`. Down to just 2 borrowers being falsely labeled as `0` (healthy).
    - This is a reduction of 65 `False Negatives` or a 97% decrease over the previous model.
    - A managable figure for a lending company, providing a much more accurate prediction on those borrowers who are in the `1` (high-risk) label.
