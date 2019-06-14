## Glue
* Glue Crawler / Data Catalog
* Data stays in S3, catalog of data is created and this can be used by RedShift,Athena, EMR  and then Visualize using 
QuickSight
* Crawlers may be easily scheduled to run periodically while defining them.
* Glue ETL runs on Apache Spark under the hood, and these happen to be the primary languages used for Spark development.
* You can run your existing Scala or Python code on AWS Glue. Simply upload the code to Amazon S3 and create one or more jobs that use that code. You can reuse the same code across multiple jobs by pointing them to the same code location on Amazon S3.
* AWS Glue outputs its progress into CloudWatch, which in turn may be integrated with the Simple Notification Service.
### Glue and S3 PArtitions
* Glue crawler will extract partions based on how your S3 data is orgnaized
* think up fornt about how you will be query your data lake in S3
* Example device send sensor dat every hours
* Do you query by time ranges - yyyy/mm/dd/deviceid
* do you query primaryly by device? - deviceid/yyyy/mm/dd
### Glue and Hive
* Hive can be integrated with Hive
* Glue datacatalog can provide metadata to Hive
* and vice versa

### Glue ETL
* Automatic code gneration
* scala or python
* encypriton
* cna be evne tdriver
* can be providion adtiona dpus to increase performancof undelry spark jobs
* errors reproted to cloudwatch

### Glue Anti patterns
* Streaming data - glue is batch oriented, minimy 5m inute intervals
* multipe etl engine - use data pipeline or emr instead fo this
* doesn't support no sql databases


### EMR (Elastic MapReduce)
* Elastic MapReduce
* Managed Hadoop frameowrk on Ec2 instances
* Master Naode : Manages the cluster , A singel EC2 instance
* Core node: hosts hdfs data and runs takes, can be scalued up and down but with some risk
* Task Node: runks taks, does not host data , no risk of data loss when removed , good use of sopost instances

### EMR USage
* Tranisetn vs Long-Running clusters
 * Can spin up task noeds using spot instance for toempary capcity
 * can use resrver isntacne on lloing running clusters tos ave $
*  Connect directly to msastewr to run jobs
* submit orderst stepd vais the console

### EMR / AWS Integration
* First of all it uses Amazon EC2 for its instances that comprise the nodes in your cluster.
* power of VPC so you can run your cluster within a virtual network for security
* ASWS S3 to store input and output data
* cloudwath tom inotro cluster
* IAM
* Croduatrail
* AWS Data piplein to scheudle and start your clusters

### EMR Stroage
* HDFS
* EMRFS : S3 as if it were HDFS
  * EMRFS Consitent View - Optional for S3 Consistence
  * Uses dyanamodB to track consitency, stores object metadata
* LFS
* EBS for HDFS

### EMR Promistes
 * EMR charges by the hours + plus EC2 changers
 * provision new nodes automatically if a core node fails
 * can add and remove taks nodes on the fly
 * can resize a running clusters core nodes

### Product Recommendations
* Exercise:
* Start a EMR Cluster : Spark , Instance Size
* cp /usr/lib/spark/examples/src/main/python/ml/als_example.py
* it is implicit that data resides on a location that is accessible by all nodes
* spark-submit als_example.py
* spark.sparkContext.setLogLevel("ERROR") // Set the log level 

### Amazon Machine Learning (ML)
* ML offers: Classification (Logistic) and Regresssion (Linear)
* How much this hous sell for?
* What is this a picture of?
* Is this biopsy result maignant?
* Is this finanacial transaction faudulent?
* Supervised Learning : Trainined on historical data
    * Label : The property we want to predict , like price
    * Training data set : sales prices, location , no of bed rooomsn
    * model : training data is used to build a model that can maek the prediction of uknown labels
    * training set : used for training the model
    * Test Set : the modle is then used to test set
    * we can then measure the accuarcy of the prdicited labesl vs their acutal lables
* What prices witll this house sell for ? : Regression
* What is this a picture of ? - Multiclass Classication, putting it in different buckets
* Is this bipsly result malignant ? Binary Classification , yes or no
* Is this finanical transaction fadudulent ? Binary Classification , yes or no
* Confustion MAtrix : True vs Prdicted label , multiclass classifcation predictive models
* HyperPRameeters : Machine learnign models often dpened on tuning the paraemetns of the model, this is call hyperparmenter tuning
* Paremets in Amzona ML include
    * Learning rate : How quickly moves on iteration to another 
    * Model Size : 
    * Number of passes : The numbe of trires it converges or not
    * Data shuffling : When data is shuffled or not
    * Regularizattion : Scaling the data to a common rate

* Provides visualation tools and wizrds to make creating a modle easy
* you point it to traingin data in s3, reshifh or rds
* it builds a modle thatn can make prodcoitin use batch oar low latecyn api
* can do traing test and evaluation your model
* fully managed
* Outdated now a days
* Usage : fraud detection, forecating product demand, personalization - prdict items a user will be interested in
* Usage : prodict user activity , classify social media
* limitatiosn : upt to 100 GB , upto 5 simulatanesou jobs (get more via support ticket)
* Anti patterns : Terabyte scal data, unsupported learning task : sequence prediction, ussupervise clustering , dep learning
* EMR / Spark is an alternative (it is unmanaged)
* Amazon ML
    * Predict Order Quantity based on order data
    * Upload the data to S3
    * Create .schema file and upload it to S3
    * Got to AWS ML and load the bucket
    * Training : The modle trains on 70% of data
    * Evaluation : Sets aside 30% for testing the model
    * Performance : gives the result of the output on the 30% teset data, 
    * RMSE : root Mean Square error : error rate %
    * Try - real time prdictions : give the value and get hte predictions
    * See the data for outliers, identifying and cleanign the data, it needs to be done before feeding it to ML



### Amazon Sage Maker
* SageMaker is a scalable solution that is both fully managed and uses Jupyter notebooks.
* Python code , Jupiter notebook
* 3 Modules, Build, Train, Deploy 
* Sage Maker is powerful
    * Tensorflow
    * Apache MXNet
    * GPU Accelerated deep learnign
    * Scaling efecitevly unlimited
    * Hyperparemet tunign jobs
* SageMaker Security:
    * Code stored in ML Stroage Volumes
        * Secuirty groups, can be encrpytped
    * All ariticate encrypted in tranist and at rest
    * API * conscole secuired by SSL
    * IAM Roles
    * Encrypted s3 buiket for data
    * KMS intergration for sage make notedbook, traing data for further securiyt
* CloudWatch, CloudTrail is intergrated with Sage Maker
* There are no fixed limits to the size of the dataset you can use for training models with Amazon SageMaker.
* Amazon SageMaker Neo : Machine learning models to train once and run anywhere in the cloud and at the edge


### Deep Learning 101
* Feedforware Nuewral Network
* Convolutional Neueral Network CNN
    * Image classification (Is there is a stop sign in this image)
* Rcurrent Newural Networks (RNNS)
    * Deals with sequence in time (predicts stock prices, undertand works ina sentenect etc)
    * LSTM (Long Short Term Memory Cell)
    * GRU (Graded Recurrent Unit)

* Deep Learnign on EC2/ EMR
    * EMR Supports Apache MXNEt and GPU Instance Types
    * AInstance typesl
        * p3: 8 teslsa
        * p2 : 16580 GPI
        * G3 : 4 M60 GPU's (all Nvidie chips)
    * Deep Learing AMI's

### AWS Data Pipeline
* Scheduling taks for processing big data
* Logfiles on EC2, Publish Those to S3, Analyze them using EMR
* Similar to Ooozie for Task scheduling
* Destinations : s3, RDS, DynamoDB, REshift anD EMR
* Manages tas ksependencies
* Trires adn notifie on failures
* corre region pileline
* Preconditon checks
* Data sources may be on onpremises
* Highly available
