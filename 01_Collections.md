#AWS BigData Speciality

## Collection
* Real time - Immediate 
  * Kinesis Data Stream
  * SQS
  * IoT
* Near Real Time
 * KDF
 * Database Migration Service
* Batch - Historical Analysis
 * Snowball
 * Data Pipeline

## AWS Kinesis 
* Kinesis is a managed alternative to Apacke Kafka
* Great for application logs, metircs, Iot, ClicksTreams,
* greate for reatime big data
* Greate for streaming processing framreworks
* Amazon Kinesis Streams 
  * Data is ingested from click stream, IOT Devices, Metrics and Logs
  * Streams are divided int ordered Shards/ Partitions
  * Data retention is 24 hours by default , can go upto 7 days
  * ability to reprorcess / replay data
  * Multiple application can consuem the same stream
  * immutable , can't be changed
* Amazone Kines Analystic
* Amazon Kinesis Firehose

### Kinesis Stream Shards
*  One stream is made of many differnet shareds
* billing is per shard provisioned
* batching avaialbe per message calls
* the number of shared can evolve over time
* Kinesis Streams REcords :
  * Data Bloc : 1MB , serialized bytes, can represetn anything
  * Reocrde Key : sent alsongsde a reocrd helps to group records in shsared
  * Sequence number : unique identifed for each record put in shareds added by kinesis after ingestion
* Producer 
  * 1 MB/s or 1000 messages/s at write per shard
  * ProvidiesThroughputExcewptions tohterwse
* Consume Clasic:
 * 2 MB/s at read per shared across asll consumers
 * 5 APO calls per shecond pers hard acorss all sconsuemr
* Consumer Enhanced Fan-Out
 * 2 MB/S at read pers hare per enhance dconsumer
 * no api call nedede (push model)
*  Data Rettion:
 * 24 hours data rention by default
 * can be extended to 7 days

### Kinesis Producers
 * Kinesis SDK
 * Kinesis Producer Library (KPL)
 * Kinesis Agent - Runs on servers , send logs reliable to kinesis teream
 * 3rd party librarier, apache Spar, Kakfa Connect, log4j
 * Kinesis Produer SDK - PutRecords(s)
  * APIUS that user use PutRecors and PutReocrds (many recors)
  * putRecores are uses batching and increases throughtput => less http request
  * providionethourhgexcceded if we go over the limits
  * +AWS Mobiel SDK
  * Use case , low thorugh, higer latency, simple API, AWS Lambada
  * Managed AWS Srouces for kinesi data stera:
   * CloudWatch Logs
   * AWS IoT
   * Kinesis Data Analytics - data can be put back into Kinesis STreams
* AWS Kinesis API - Excpetions
 * ProvisionedThroughputExceeded Exceptions
  * Happends when sending more data (exceeding MB/s or TS for any shard)
  * Make sure you dont have a haor shard
* Solution :
 * Retires with backoff
 * increase the shards
 * get the partition key correct
* Kinesis Producer Libarary (KPL)
 * Esasy to use and high ocnfiguratbl C++/Java
 * used for buiklding high performance, long running producers
 * automated and configurable retry maechansim
 * sysnchornous and asynchrons api (better perfomrance for asyn)
 * submits metrics to cloudwath cformonitorg
 * batching : both turned by default - increas throughput, decares cost
  * collect Recors and write tom ultpe shares in the samse putrecods api call
  * Aggregate - increased latency
   * capabilyt sto store muylte recrds in ore recode
   * increas paylod size and improve thorughput
 * RecordMaxBufferedTime - default 100 ms , aggreate into one reocrds <1 MB , Collection
* compression must be impemetend by the user
* KPL Recorss muyst be decoded with the KCL or special hlper libray
* Kinesis Agent
  * Monitr log file ansd send them to kinesi data streams
  * java bease age  bui8l ton to KPL
  * install on linch based server environments
  * Features :
   * Write from multple directions and wirte to multiple stream
   * routing featue base on director log fiel
   * pre process data before seind go streams ()
   * the agend handles file rotation, checkpointing and retyr upon failures
   * emit metris to cloudwath for monitoring

### Kinesis Conumers - Classic
 * Kinesis SDK 
   * GetReocrds
   * Classic Kinessis - Reocrs are pooled by consume from a shard
   * Each Shard has 2 MB Total aggreate trohgoput
   * Get Records reutns upto 10 MB of data (then throttle for 5 seconds) or upt to 10000 records
   * Maximu of 5 GetReocrs API Calls pershard per second = 200 ms latency
   * if 5 consumes application conume from the shame shard, emans every consume can ppoll once a second and recieve les than 400 KB/S
 * Kinesis Client Library
   * Java -fris libray but exiss fo other language
   * Read recodrs frim kinesis produced with kpl de aggreateion
   * share multipel shared with multple consume in one group shard discover
   * checkpoinging feature to resume progress
   * Amazzon dyanmod is used for checkpoing progress
    * make sure enought WCU / RCU
    * or use on demand for DyanmodDB
    * otherwise DyamanoDB may slow down KCL
   * Record processow will process the data
   * Kinesis connector libray : old ljava librayry, deprecated 
 * Kinesis Connector Librarary
 * 3rd party librarier, Spark, log4j, appendeders ,flume ,kafka connect
 * Kinesis firehouse
 * AWA Lambda
   * AWS Lambda can soure reocrds from kinesis data stream
   * lambda consume has library to de-aggreat recordin from the KPL
   * lambda cna vue used ro tun ligh weht elt
    * Amzons s3
    * dyanmod b
    * red hshif
    * leaslti search
  * Lambda can bue sued to rigger notifacton .send emai in reamtai
  * configurable batch size
* Kinesis Enhance Fan out 
   * new feathcure
   * works iwth KCL 2.0 abd AWS lambda
   * Each consue get 2 MB/S of providinoed thoughput pershard
   * That mean 20 conumesr wil lget 40 MB.s per shared aggregated
   * push based not pull shared , pushed over http/2
   * no more 2MB/S limit
   * Reduce latency (!70ms)

### Kinesis Operations - 
* Adding shards
 * Also called shared splitting
 * can be used to increase the stream capcity 
 * can be used to dived a hot shard
 * the old shard is closed and wil lbe delete once the data is expired
* Meging Shards
 * Decreas  the stream capcity and ave costs
 * can be used to gorup tow shared with low traffice
 * old shared ared closed and dleted basedo ndata expiration
* Auto Scaling
 * Not an tntive feature
 * API Call to change the number of shards id UpdatedShardCount
 * We can uimplement using AWS LAmbda usinga blog
* Limitations of Scaling
 * Resharding cannot be done in paralller. plan acapity in advance
 * you can only perfomr one reshard operation as time and it takes a few seconds
 * for 1000 shard it take 30k seconds to doubth ethe sarhed to 2000

### Kinesis Security
 * Control access / authorization using IAM Polices
 * encryptioni n flight using https endpoint
 * encryption at rest using kms
 * clinet side encryption must be manually implmeented
 * VPC endpoints avaialbe for  kinesis to access within VPC

### AWS Kinesis Data Firehose
 * Fully Manage SErvice, no admisnitration
 * Near REal time (60 seconds latency miniium for non full batches)
 * Load data into redshift / s3 / elatsit search / splukn
 * Automatic scaling
 * support many formats
 * data conversion form jsodn to prquet/orc
 * data transfomrathi thoug hasw lambe (*csv - josn)
 * support somcpcression when target is amaonze s3 (gzip,gaxip and snappy)
 * only gzip isd the dat furhter laoded into redshfit
 * pay for the mamoutn data oign thorugh firehouse
 * Spark / KCL do not read from KDF
 * SDK (KPL), Kingesi Agen, Kines Dats Stream, CludeWatch Logs Even ,IOT Rules ====> Kines Data Fireshose ====> S3, redhshfit,elastic seach, splunk
 * Data can be trnasfomred using AWS lambda
 * Source Reocrdd, Trnasofmration failure , deliver failures ===> Amazon S3 Bucket
 * Fireshose accumlates rcordsi n a  buffger
 * the ubffer is flushed basedo n time ansd ize rules
 * Buffer Size
 * Buffer Time
 * Fireshose can cautolmaticaly infreas the buffer sizse to increst theorughput
 * High through ==> buffer size
 * Low Trhough ==> Buffer time will hit
### Kinesis Data Stream vs Fireshose
* Streams :
 * Write custome code 
 * Real time 200 ms , 70 ms for enchacne fanout
 * must manage scaling
 * data storegae for 1 to 7 days
 * use with labmd to isnert dat in reamte to elasterach
* fireshose
 * full manage, send to
 * serveless data transofmrat iwth lambad
 * nea relat time
 * automated scaling
 * no datas storage

### Excercise Kinesis Firehose, Kinesis Streams

### AWS SQS
 * Oldest offering 
 * fully managed
 * scalade form 1 message per seconds to 10,0000s per second
 * defaul retaiton of message : 4 days m maximu of 14 days
 * no limit to howm manay message can be inth e queue
 * low latency (<10 ms on publish and receive>)
 * Hoirizatnat scaling in terms of number of sconumsers
 * can have duplicatem essages ( at lest once deliver, oaccasionally)
 * can haveo ut of order message ( best effor oderding)
 * Limation of 256KB Per message sent
 * Consulers polls SQS messages, process and delete messages
 * AWS SQS - FIFO Queue
  * new offering 
  * name of the queue must end in .fifo
  * low throughoutput
  * meessages are preocessed i norder by the consumes
  * mesage are esent exeactly once
  * 5-munte interval de-duplication
* SQL Extended Client
 * Mesage size  , sent laarge mesage using S3
* Use CAses
 * Decoupe applications : To handle payements asynchronously
 * Buffer writes toda databse : for example a voting application
 * Handle larget load of messages comming in
* SQS Limits
  * Maxmimu of 120,000 in flight messages being processed by consumers
  * batch request has am aximim of 10 message - max 25k kb
  * message content is xml , Json , unformtated test
  * Stnadare queues have an ulinmited tPS
  * FIFO queue support upt to 3000 messages per second
  * max mesasge is 256 KB
  * Data retation form 1 min to 14 days
  * pricing :
    *  pay per api request
    * pay per network usage
* SQS Security
 * Encypriton in flighut using ht tthps endoint
 * Can eanble SEE using kms
  * can set the CM we want to use
  * SSE only encryptd th body not hte metada
 * IAM policy msust allow usage of SQL
 * SQL queeu accessp olkcy
  * fine grainger access policy
  * 
### Kines Data STream vs SQS
* Kinesis Data Tream :
  * Data cna be consume man yitmes
  * data is deleted after the retaion period
  * pordering of redcos is pesrevers
  * build multipe application reading tfomr the samd strea mindependly
  * Streaming maprequest querycin capbilty
  * checkpoing neede to raack progrss of consumption
  * shared must be provided ahead of time
* SQL
 * Queue, decoupe applicaiton
 * one application preq uqeue
 * records are delted afte consusmption
 * mesage are process independly for stand queure
 * OPder for fifo queue
 * capcoitl to dalye message
 * dyanmaci scaling of lad 
 

 ### IoT Overview
  * We deploy IoT devices ("Things")
  * We configure them and tertive data from them
  * IoT Thing => Device Gateway => Things Registry  , Device Shadow, Iot Message Broker => IOT Rules Engines => Kinesis, SQS , Lambda etc
  #### IOT Core
   * IOT Device Gateway
    * Servies as th entryp oint for IoT devices conneting to AWS
    * allows devices to securite and effieciently communicati nowith AWS IOT
    * Support the MSQTT, WEbscokes and HTTP 1.1 protocal
    * fully manages adn dcaled automatically to suport over a billing deivces
    * no need to manage any infrsttucte
   * Message Broker
    * Pub/Sub message pattery - low latency
    * Device can comunciatie with one another this wasy
    * mesages sent using MSQTT, websocker or http 1.1
    * mesages are publicsh inot topic just like sns
    * message borker forware messgate to all clients subscribe
* IOT Thing REgistry = IQA of IOt
 * All oncnecte iot deivce sare repesetnese with asws regsit
 * organize resourlces
  * each device get a unique id
  ( suppror mestaa)
  * can crate x.509 certifiacte 
  * IOT Groups 
* Authentication
 * 3 possible abuther tmethdo f thing
  * crat x.509 certifacate and laod securei
  * aws sigv4
  * custom tokens with custom autorize
* For mobile paps
 * Cognito Idnetieds
* Web /Desktop /CLO 
 * IAM
 * Federated ednties

* Device Shadow
 * JSond document represent the state of cconnectin thing
 * state can be set ado dfierent desried state
 * the iot hting wil lretieve thstate when online and adapt
 * Ysnychinozation of stat will happend whtn the lightbuld is on
* Rules Engine
 * Rules are defineo n the MSQTT Topics
 * Rules = when it s triggers 
 * Action = what is does
 * Rules use cases : Kinesi, Dyamnod,sql,sns,s3,lamda
 * Rules need IAM roles to perfomr their actions
* IOT Green grass
 * IOT Greengrass bingd the computer layer to hte device direclty
 * you can exeucte asws lamdgad function on the deivces 
   * pre process the data
   * exeucte prdiction base on md lodel
   * keep deivce data in sync
   * communciation betwe nl ocad deivce
 * operation offline
 * deploye funcitons form the cloude directly to the d4evi e

### DMS - Database Migration Services
 * Quick and secure migrate database to aws reslieeltn and selfh eallintg
 * the source datacagse remaing avaiald during htem irgation
 * Supports : homogenous, heterogenous
 * Continous data replication using EC2
 * Sources : On presmie, ec3 instance databse ,oracle , ms sql server ,mysql,mariabled b,p sotgre , mondgo, sap ,db,s3, azure sql, aurora,
 * Detintation
 * AWS Schema Convesion ToolS (SCT) - Conver your database from eone eninge to anthoer
 *
