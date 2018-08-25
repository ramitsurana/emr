# Menu

* [EMR](#emr)
* [Machine Learning](#machine-learning)

## EMR

## Machine Learning

#### Basics

* Schema - Different Columns/classes obtained from Data Source
* Target - The Class/Column to select for ML to learn and do analysis from
* Row Identifier (Optional) 


#### Use Data Source as 

* Create (train) an ML Model
* Evaluate an ML model
* Generate Batch predictions

##### Create (train) an ML Model -

Under training and evaluation settings, we can select Default Settings or Custom Settings -> Create a ML Model

##### Evaluate an ML model -

- Automatically gets created and generates Histogram after the create (train) ML model is created
- Meterics will appear such as AUC for Binary, Confusion matrix etc.
- Can adjust Threshold depending on Business Requirements

AWS Machine learning Models are -

| Models   |      Purpose      |  Example | Algorithm | Measuring Unit for Quality of Histogram |
|----------|:-------------:|:---------|:----------------|-----------------|
| [Binary Classification Model](https://aws.amazon.com/blogs/big-data/building-a-binary-classification-model-with-amazon-machine-learning-and-amazon-redshift/) |  Predict a binary outcome (one of two possible classes) |  "Is this email spam or not spam?" |  Logistic regression | AUC(Area Under Curve) [Higher the value, better the quality] |
| [Multiclass Classification Model](https://aws.amazon.com/blogs/big-data/building-a-multi-class-ml-model-with-amazon-machine-learning/) | multiple classes (predict one of more than two outcomes) | "Is this product a book, movie, or clothing?"| multinomial logistic regression | Confusion Matrix |
| [Regression Model](https://aws.amazon.com/blogs/big-data/building-a-numeric-regression-model-with-amazon-machine-learning/) | predict a numeric value. | "What price will this house sell for?" | Linear regression | RMSE [Lower the value, better the quality] |

Ref - https://docs.aws.amazon.com/machine-learning/latest/dg/types-of-ml-models.html

 0 - Most Likely
 1 - Unlikely 
 
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

#### Batch Prediction -

Obtains output of a data source batch and creates a csv file with results into S3.Output CSV will contain sample like -

| Best Answer |   Score  |
|----------|:-------------:|
| 0 |   0.32 |
| 1 | 0.98 |

Here the Best Answer will be 0 or 1, depending upon the threshold value set by user in the settings.

#### Real Time Predictions

Under Evaluations -> Try-Real Time Predictions -> Paste a Record
