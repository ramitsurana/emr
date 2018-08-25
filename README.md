# Menu

* [EMR](#emr)
* [Machine Learning](#machine-learning)

## EMR

## Machine Learning

AWS Machine learning Models are -

| Models   |      Purpose      |  Example | Algorithm | Prediction Score |
|----------|:-------------:|:---------|:----------------|-----------------|
| Binary Classification Model |  Predict a binary outcome (one of two possible classes) |  "Is this email spam or not spam?" |  Logistic regression | 0 - Most Likely,   1 - Unlikely |
| Multiclass Classification Model | multiple classes (predict one of more than two outcomes) | "Is this product a book, movie, or clothing?"| multinomial logistic regression | - |
| Regression Model | predict a numeric value. | "What price will this house sell for?" | Linear regression | - |

Ref - https://docs.aws.amazon.com/machine-learning/latest/dg/types-of-ml-models.html

* Confusion Matrix -

The confusion matrix gives some insights about the performance of the model as a way to visualize the accuracy of multiclass classification predictive models. The confusion matrix illustrates in a table the number or percentage of correct and incorrect predictions for each class by comparing an observation’s predicted class and its true class.

Blue Color - Correct Predictions
Light Brownish Color - Incorrect Predictions

* F1 Measurment -

F1 measures the quality of the model. Ranges from 0 to 1. Higher the F1 Score, better the Quality of ML model.


* AUC (Area Under Curve) (Histograms) -

If the value is near to 1 or above 0.5, then the binary model prediction is correct else high chances of it being wrong.It comes under **default** settings. It measures prediction accuracy.

* Trade Off Threshold 

The trade off threshold value provides a business approach to alter the false positives and negatives based on a business descision from the output of the outcome graph.

Ref - https://aws.amazon.com/blogs/big-data/building-a-multi-class-ml-model-with-amazon-machine-learning/

#### Batch Prediction -

Obtains output of a data source batch and creates a csv file with results into S3.Output CSV will contain sample like -

| Best Answer |   Score  |
|----------|:-------------:|
| 0 |   0.32 |
| 1 | 0.98 |

Here the Best Answer will be 0 or 1, depending upon the threshold value set by user in the settings.

#### Real Time Predictions

Under Evaluations -> Try-Real Time Predictions -> Paste a Record





