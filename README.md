# Menu

* [EMR](#emr)
* [Machine Learning](#machine-learning)
* [Notebooks](#notebooks)

## EMR

#### EMR File Storage and Compression

|  Algorithm/Compression Type   |      Splittable   | Compression Ratio | Compress_Decompress Speed |
|----------|:-------------:|----------------:|--------------------|
| GZIP |  No | High | Medium |
| bzip2 | Yes | Very High | Slow |
| LZO | Yes | Low | Fast |
| Snappy | No | Low | Very Fast |


#### HBase vs Dynamodb

|  HBase   |      Dynamodb   |
|----------|:-------------:|
| Wide Column Store | Key-Value Store |
| No row size restrictions | Item Size Restricted |
| Flexible Row Key Data Types | Scalar Types |
|Index Creation is more manual | Easier Index Creation |

#### View Web Interfaces Hosted on Amazon EMR Clusters

|  Name of interface	 |      URI   |
|----------|:-------------:|
| YARN ResourceManager	 | http://master-public-dns-name:8088/ |
| YARN NodeManager | http://slave-public-dns-name:8042/ |
| Hadoop HDFS NameNode |	http://master-public-dns-name:50070/ |
| Hadoop HDFS DataNode |	http://slave-public-dns-name:50075/ |
| Spark HistoryServer |	http://master-public-dns-name:18080/ |
| Zeppelin	| http://master-public-dns-name:8890/ |
| Hue	| http://master-public-dns-name:8888/ |
| Ganglia	| http://master-public-dns-name/ganglia/ |
| HBase UI |	http://master-public-dns-name:16010/ |


#### Long Running vs Transient Cluster

|  Long Running Cluster   |   Transient Cluster |
|----------|:-------------|
| Cluster stays up and runningfor queries against HBase | Temporary Cluster  |
| Jobs in cluster run frequently | Shouts Down after Processing |
| Data may be large | Batch jobs when needed |
| Keeps HDFS Data on Core Nodes | Input,Output and Code Stored in S3 |
| Termination Protection | Easy Recovery |
| - | Hive metastore stored in MySQL RDS |
| - |  Reduces Costs |

#### EMR Cluster Differences b/w Large Nodes, Small Cluster and Small Nodes, Large Cluster

|  Large Nodes and Small Cluster   | Small Nodes and Large Cluster  |
|----------|:-------------|
| 500 Large Nodes | 750 small nodes  |
| AWS recommends a small cluster with large nodes due to low maintanence | It requires High maintanence |


#### Replication factor

* Default replication factor for EMR

- For a cluster of 10 nodes or more = 3
- For a cluster of 4 - 9 nodes = 2
- For a cluster of 3 nodes or less = 1

Setting can be found in **hdfs-site.xml**

** Use HDFS for high I/O requirements.**

#### HDFS Capacity

```
For example, 10 nodes * 800 GB = 8 TB

For a Replication Factor of 3.

HDFS capacity => 8 TB/3 = 2.6 TB

Comparing the Data Size and HDFS capcity, we get,

Data Size => 3 TB
HDFS Capacity => 2.6 TB

Conclusion is NOT enough Space
```

#### Hue

Hue UI Browser

- Amazon S3 and Hadoop File System (HDFS) Browser
- With the appropriate permissions, you can browse and move data between the ephemeral HDFS storage and S3 buckets belonging to your account.
- By default, superusers in Hue can access all files that Amazon EMR IAM roles are allowed to access. Newly created users do not automatically have permissions to access the Amazon S3 filebrowser and must have the filebrowser.s3_access permissions enabled for their group.
- Hive—Run interactive queries on your data. This is also a useful way to prototype programmatic or batched querying.
- Pig—Run scripts on your data or issue interactive commands.
- Oozie—Create and monitor Oozie workflows.
- Metastore Manager—View and manipulate the contents of the Hive metastore (import/create, drop, and so on).
- Job browser—See the status of your submitted Hadoop jobs.
- User management—Manage Hue user accounts and integrate LDAP users with Hue.
- AWS Samples—There are several "ready-to-run" examples that process sample data from various AWS services using applications in Hue. When you log in to Hue, you are taken to the Hue Home application where the samples are pre-installed.
- Livy Server is supported only in Amazon EMR version 5.9.0 and later.
- To use the Hue Notebook for Spark, you must install Hue with Livy and Spark. 

Ref - https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hue.html
LDAP Integration - https://docs.aws.amazon.com/emr/latest/ReleaseGuide/hue-ldap.html

#### Presto

Advantages -

- High Concurrency , can run thousands of queries per day
- In-memory processing
- Low I/O and latency
- Can run query of different types from sources such as RDBMS, NoSQL DB's & Frameworks like Hive, Stream processing & Kafka
- No need of interpreter layer like Hive does (Tez)

Disadvantages -

- Not designed for OLTP
- Joining very large (100M+) rows not possible, use Hive intead
- Not able to perform Batch Processing

#### Hadoop Encryped Shuffle

Data in-transit between nodes is encrypted.

This process involves transferring data from node to node within the cluster, and if you want that data to be encrypted in-transit between nodes, then Hadoop encrypted shuffle has to be setup. Encrypted Shuffle capability allows encryption of the MapReduce shuffle using HTTPS. When you select the in-transit encryption checkbox in the EMR security configuration, Hadoop Encrypted Shuffle is automatically setup for you upon cluster launch.

It allows encryption of the MapReduce shuffle using HTTPS and with optional client authentication(HTTPS with client certificates). It includes -

* Hadoop configuration for shuffling between HTTP and HTTPS
* Re-load trust certificates across the cluster
* Hadoop Configuration settings for specifying keystore and truststore properties

For enabling the shuffle, the hadoop files need to changed as per the properties file mentioned in the link below.

Ref - https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/EncryptedShuffle.html

#### Library used for Kinesis Steaming

- KCL(Kinesis Client Library) 

### Spark

- Not used for batch processing.
- Uses In-memory Storage
- Avoid using it for multi-user reporting with many concurrent requests
- Spark’s four modules are MLlib, SparkSQL, Spark Streaming, and GraphX.

**Note** 

1. EBS volumes get deleted once EMR cluster is terminated.
2. (D2 and I3 instance types)Instance storage can be used for HDFS if the I/O requirements of the EMR cluster are high.
3. Once you enable encryption for a Redshift cluster upon launch, you can cannot then change it to an unencrypted cluster. You’ll have to unload the data and reload the data into a new cluster with your new encryption setting.

#### EMR Encryption

- LUKS Encryption

**Condition**

We need to use EMR with EMRFS. However, your security team requires that you both encrypt all data before sending it to S3 and that you maintain the keys.

**Answer**
Would use CSE-Custom, where you would encrypt the data before sending it to S3 and also manage the the client-side master key. The other encryption options available are: S3 Server-Side Encryption (SSE-S3), S3 manages keys for you; Server-Side Encryption with KMS–Managed Keys (SSE-KMS), S3 uses a customer master key that is managed in the Key Management Service to encrypt and decrypt the data before saving it to an S3 bucket; Client-Side Encryption with KMS-Managed Keys (CSE-KMS), the EMR cluster uses a customer master key to encrypt data before sending it to Amazon S3 for storage and to decrypt the data after it is downloaded.

## Machine Learning

Types of ML-

* Supervised Learning - Requires Labelled Data and Desired Output
* Unsupervised Learning

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

#### AWS Supervised Machine learning Models are -

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

## Notebooks

**Zeppelin**

- Web based notebook 
- Integrates with S3
- Publish to dashboards
- Can use scala, python, spark sql and hive sql
- Dashboards can be shared with other users
- Uses Spark settings on EMR

**Jupyter**

- Python based notebooks

**D3.js**

- Javascript Library 
- Read Data from tsv, csv or json files
- Generates HTML Tables, SVG bar charts and other dashboards etc.
