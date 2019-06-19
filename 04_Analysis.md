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


