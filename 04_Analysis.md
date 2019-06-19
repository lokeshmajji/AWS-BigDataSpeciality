# Analysis

## Amoazon Kinesis Analytics
* Kinesis Analytics can only monitor streams from Kinesis, but both data streams and Firehose are supported.
* While you might in turn connect S3, Redshift, or Lambda to your Kinesis Analytics output stream, Kinesis Analytics must have a stream as its input, and a stream as its output.
* If a record arrives late to your application during stream processing , records is written to hte error stream
* Kinesis Data Analytics provisions capacity in the form of Kinesis Processing Units (KPU). A single KPU provides you with the memory (4 GB) and corresponding computing and networking. The default limit for KPUs for your application is 8. 8*4 = 32 GB
* 

* Create Kinesis Stream
    * Create Kinesis Stream : OrderRateAlarms
    * Number of Shards : 1
* Data Anlytics :
    * Create Application : TranactionRateMonitor
    * Runtime : SQL
    * Connect Streaming Data
    * Select CadabraOrders
    * Kinesis : Discover Schema
    * Launc the log generator script
    * tail -f /usr/log/kinesisream/logs
* Real time analytics
    * copy the sql script from the course 
    * save and run sql 
* Connect to destinations
    * Select the kinesis stream
    * Select in application stream
    * select output format : json
    * save and continue
* IAM : Create a Role : LambdaKinesisSNS
    * lambdasrvice
        * AWSLambdaKinesisExecutionRole
        * AWSSNSFullAccess
        * CloudeWatchLogsFullAccess
        * AWSLambdaBasicExeuctionRoles
* Labmda : Creation Function
    * Name,
    * Runtime : Python 2.7
    * Chooese the reole
    * Kinesis
        * OrderRate Alarms
        * set time to 1 min
    * Copy the python lambda function
    * Create SNS Topic
* NOTE : Python Script -> Logs -> Kinesis Agent -> Kinesis Stream -> Kinesis Analytics -> Kinesis Stream -> Lambada Function -> SNS Topic - Message to Mobile

## Elastic Search
* Petabyte scala anlaysis and reporting
* Elastic Stack
    * Fundamentially a search enginer
    * An analysis tool
    * Scalable version of Lucene(open source tool)
    * A visualizatio toool (Kibana)
    * A data pileline (Beast/ LogStash)
        * Use Kinesis Too
    * Horizationally scalable
* Use cases:
    * Search Engine for a website - Indexing for UK Web Archives for searching
    * Full text search : MirrorWeb makes UK parliament Web Archives searchables
    * Log Analytics : Adobe uses, 3000 API calls for second, traffic patterns
    * Application Monitoring : Expedia, RCA and price optimizations, docker logs
    * Seuciryt Anlystics : Index and analyze the data from the enterprise quickly
    * ClickStream anlytics : 30 TB data of processing worldwide websites , near real time
* ElasticSeach Concepts
    * Documents : Storage and retreival , every document has a unique id and a type
    * types : scheman and mapping shared by documennt that represt the same sor of athing : getting obsolete
    * indices : an index power search into all document with an coolecito of types. They contain inverted incides that lety ou seach accross everythign iwth htem at once
    *  An index is split ito shards
    * every document is placed in a shard
    * Each shared may beon a differnete node in a cluster
    * Evyery shared is a self containt Lucene index of its own
    * Reduncdany : The indes has tow proviem ary sharedn adn two replicas , you applicatin shoud round robin reuest amont nodes
    * Write request are sent to primary and adn then replicated to replciates

## Amaozn ElastiscSeach sERvice
 * Fully Manageed
 * Scale up or down withou down time
    * Not autoatic
 * Pay for what you use
    * Instancehours, storage , data transfer
 * Network isloation
    * Amzona VPIC, Encyption, Cognitor, IAM,
 * AWS Integration
    * S3 bcuketrs
    * Kinesi data streams
    * dyanmob db stream
    * Cloudwatch / cloudtrail
    * zone awreness
* Dedicate master nodes
    * chocie fo cuohtn and instacne types
    * domains : similar to cluster in AWS
    * snapshot to S3 : snapshot is created 
    * Zone awarenesss : for higerh elatcny
* ES Security
    * Resourece based prolices
    * idnetiy based polices : IAM
    * IP based polices
    * REquest siging : all requests must be digitally signed, otherwise not encrypted
    * VPC
    * Cognito
        * Nginx reverse proxy on EC2 forwared to ES domai
        * SSH tunnes to f fort 5601
        * VPC Direct connect
        * VPN
* Anit Patterns
    * OLTP
        * No transactions
        * RDS or DyanmoDB is better
    * Ad-hoc data quertying
        * Athena is better
    * Remember Amzona ES is primaory fro search and anlytics

### Amazon Elatisc Search Service Part 1
* EC2 Instance -> Create Server Logs and Dump into Kinesis Data Firehose -> Amazon ElasticSearch Service
* How can you ensure maximum security for your Amazon ES cluster?
* Kinesis, DynamoDB, Logstash / Beats, and Elasticsearch's native API's offer means to import data into Amazon ES.
* Amazon ES created daily snapshots to S3 by default, and you can create them more often if you wish.


## Athena
* Interactive query service for S3 (SQL)
* Prest under the hood
* Serverless
* Supports
    * csv : Human readable
    * json : Human readable
    * orc : columnar, splittable
    * parquet : columnar, splittable
    * avro : splittable, schema based
* Unstructured, semi-structured, or structured
* use cases:
    * Ad-hoc queries of web logs
    * querying staging data before loading to Redshift
    * analyze CloudTrail/ CloudFront / VPC / ELB etcs logs in S3
    * Intergration with Jupyter,Zeppelin,Restudio notebooks
    * Integration with QuickSight (Visualization tool)
    * Integration vai ODBC/JDBC with other visualaiztion tools
### Athena + Glue
    * Glue crawler extract schmea from S3 and create a catalog
    * Athena sees the catalog and creates table and query using SQL. RDS, EMR, Hive Metastore can use Glue Data Catalog
    * Quicksight can be used to visualize
* Cost Model:
    * Pay-as-you-go
        * $5 per TB Scanned
        * succesfu or cancelled queries count ,faile quered do not
        * no charge for DDL
    * Save LOTS of money using columnar formats
        * ORC, Parquet
        * Save 30-90% and get better performance
    * Glue and S3 have theri own chargers
    * Partitioning your data in S3 will save yours costons in Athena
* Security:
    * Access Control
        * IA< ACLS, S3 bucket polices
        * AmazonAthernaFullAcess/ AWSQuickSightAthenaAccess
    * Encryp results at rest in S3 staging directory
        * SSE - S3
        * SSSE - KMS
        * CSE - KMS
    * Corss Account acces in S3 bucket policy possible
    * Trnasport Layter Security encryptes in transit(between Athenan and S3)
* Anit Patters:
    * Highly formatted reports / visualization , use quicksign instead
    * ETL 
        * Use Glude Instead
        * Or spark
### Excercise:
    Server Logs -> Kinesi Data Fireshosue -> S3 -> Glue -> Athena

* Glue -> Crawlers -> Add Crawler
* Choose datastore
* exclude patterns
* create an IAM Role
* Frequency : Run on Demand
* Create database for storing the output data
* Edit the schema if rquired
* Goto Athena, Databases and Tables should alread ybe present 
* select description,count(8) from orderslogs where year='2019' and month = '2' and country='France' group by description

* As a Big Data analyst, you need to query/analyze data from a set of CSV files stored in S3. Which of the following serverless services helps you with this?
* Using columnar formats such as ORC and Parquet can reduce costs 30-90%, while improving performance at the same time.
* When using Athena, you are charged separately for using the AWS Glue Data Catalog. True

## Amazon Redshift
* Fully Managed , Petabyte Scale Datewarehouse service
* 10x better peerfomance than other DWS's
    * via machine learning
    * MPP (Massively Parallel Processing )query execution
    * Columnar storage
    * Column compression
* Desinged for OLAP not LTP
* Cost effective
* SQL, ODC, JDBC Interfaces
* Scale up or down on demand
* buit in replication and backups
* moinotr via Cloudwatch / CloudTrail
* Resdshift used 1MB Block Size
* Automatic sampling the data for chosing the better compression


### Redshift Spectrum
    * Query Exabytes of unstructe data in S3 withou laoding
    * limitless concurrency
    * Horizantal Scaling
    * Separate storgage & compute resources
    * Wide variety of data formats
    * supports csv,text,tsv, orc,parquet,avro,rc
    * Support of Gzip and Snappy compression

* Reshuft Durablity
    * Replication wihtin cluster
    * Backup to S3 - Asynchonly replicated to another retgion
    * Automated snapshost
    * Failed drivers / nodes automatically replcated
    * Howeever limited to a single AZ
* Scaling
    * Vertical and Horionata scaling on deoman
    * during scaling
        * A new cluster isc rated whiel your old one remaina aviale for reads
        * cnane is fliped to new cluster
        * data moved in paraller to new compute nodes
* Reshsift distriubtion Styles
    * Auto : Reshifut figured it ou base on size of data
    * Even : Rows distributed across slices in a round-robin
    * Key : ROWS distributed base on one Column , makes keys phyisicall located in same places
    * ALL: Loads all data into a single location to all nodes.
    * It is not possible to chang the distribution style once it is created
* Sort Keys
    * Rows are stored on disk in sorted order based on the column your sesingate a as sort key
    * Like an index
    * Makes for fast range queries
    * Choosing a sort key (Stores min and max colums)
        * Recencey - Timestamp
        * Filtering - Range or Equality Filtering
        * Joins - use the column which is used for joining
    * Sort Keys : Single Column : Date as a single column
    * Sort Keys : Compound Column : All the columns in a table in the order they are present
        * they ehlp in compression
    * Sort Keys : Interleaved
        * equal wieght to each column, this is useful joins are used in a different columns

### Importing / Exporting Data
    * Copy command
        * praalleize, efficient
        * from S3, EMR, DynamoDB, remote hosts
        * S3 requires a minite fiele and IAM Roles
    * UNLOAD Command
        * Unload from a table into files in S3
    * Enhanced VPC routing

* Redshift copy granst for coress region sanpshot coipies
    * les sa you have akms encrypte reshfit cluster and a snspahso of it
    * if you want ocopy that snaptho to another tgion for gackkup
    * In the destiona aw sregion
        * crea ak kms key if you don thave on laready
        * specif a uniqu nae for your snapc sh copy grant
        * specif the kmsk ey id for which are creating the copy gratn
    * In teh source : enabley copying of snaphsot to the copy grant you just created
* DBLINK
    * connect Reshift to PostgreSQL
    * good way to copy and syn data between PostGre SQL and REdshuft

* Integraion with other services
    * S3
    * DynamoDB
    * EMR / EC2
    * Data pipeline
    * Data Migraiton Service
* Redshift Wrokload Management (WLM)
    * Prioritze short , fast queries vs long, slow queries
    * Query queues
    * via console, CLI or API
* VACUUM Command
    * Recovers space from delted rows
        * VACUUM FULL - Sort and reclaim space
        * VACUUM DELETE ONLY - Reclaim space
        * VACUUM SORT ONLY - sort only
        * VACUUM RE-INDEX

### Anti Patterns
    * Small data sets - use RDS instead
    * OLTP : use RDS or DyanmodB Instaed
    * Unstructed DAta
        * ETL First with EMR etc
    * BLOB data
        * store referece to large binary files in S3, not the files themselves
