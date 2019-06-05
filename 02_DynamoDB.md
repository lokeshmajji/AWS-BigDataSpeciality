# DynamoDB
* Fully managed, Highly vavialbe wiht replicatess 3 AZ
* NoSQL database 
* Scales tom assive worklaod ,distribute dtabase 
* Low latency
* millions of request pers sconeds ,trillions fo rwo
* Inegrati with IAM for secuirty,
### Basics
* Dynamod is am ad of tables
* each table has a primary key (must be deiced at cration tamie)
* deach talbe have an infinte noumbe rof itmes
* each imte has attirbutes 
* maixmu size of an itmes 400 KB
* Data Type: String , Number, Binary, Boolean, Null
* List , Map
### Primary Keys
 * partiotin key only (HASH) - Userd id in user table
 * Partition + Sor Key - data is grouped byp artition keys, sort keey == range key
 * User-game table, user id of the parition key ,game_id for the sor key
 * movide database - movie_id is primary key, high cardinality
 * USe cases : mobie paps, gaming ,digait ad server, live votintm, senso netowrk. log insgetion, aweb sesion management
 * BLOB , stored the object in S3 and metadata in DyanmoDB
### Provisioned Throughput
* RCU - Read capcity units
* WCU : Throughput for wirtes
* Option to setup  auto sacling of thorugput to meet deaman
* thorugh can be exceedded tompraoriy using burst credit
* if bust credist are empty, you will get ap rovisidethoroughputexception
* WCU -Write capcity unit
* One wirte cpacit yunit represet one wiret per seconcd for an itme to 1 KB ins zine
* if the items are tlareget thane 1 kb more wcu are consudme
* We write 120 objecst per minute of 2kb each = (120 (Objects) /60 (Seconds)) * 2 (KB) = 4 WCU

### Strongly consitent vs Eventually Consistent REad
 * by default , eventually consisten readys
 * due to replication, write to a server doesn't guarentte that you get reads from servers
 * One Read capicyt unit repsresnt one strongly consigetn read per sconed, or two enveutally soncisdent reads pers deoncd for anu item of 4kb ins Size
 * if the iterma lare lare 4KB ,more RCU are conusme
 * 10 strongly consiten read per second of 4 KB Each = 10 * 4 KB / 4 KB = 10 RCU
 * 16 evnetualy consident reads per second of 12 KB Each
 * (16/2) * (12/4) = 24 RCU
 * 10 strongly consiten read pers deoncd of 6KB Each
   10 * 8 KB / 4 KB = 20 RCU // 6 kb rounded to 8kb

### Throttling
* If we exceed our RCU or WCU m we get ProovisioneThroughPutExceededExceptions
* REasons : Hot keys / partions
* Very large items
* Solution : Exponsntial back off when exception is cencouderd
* Distrubte paritin keys as much as poosibbe
* If RCU issue ,w ecan DynamodDB Accelartion (DAX)

### Partitions
* You cnas stit with one partition
* Each partition : Max of 3000 RCU / 1000 WCU , Max of 10 GB
* WCU and RCU are spread eveny between partitnos

### Writing Data
* PutItem
* UpdateItem
* ConditionalWrites
* DeleteItem
* DeleteTable
* BatchWrites - Upto 25 PutItem / DeleteItem in once call
* upto 16 MB or data writter or upto 400 kb of dta per item

### Get Item
 * Read based on primary key - Hash or Hash-Range
 * Enventually consident by derfault
 * Stronlgy consitent can take more time and more RCU

 ### Query
 * PartityKey value (= only operator)
 * sorkey (=,<,<=,>,>=, Between, In, Beginwith)
 * the filter can be applied only on the client side not on the dyanmodb side

 ### DyanmoDB Scan
 * Scan the entire table and then filter out data(inefficient)
 * Returns up to 1MB of data - use pagination to keep on reading
 * Consulmes a lof of RCE
 * limit impact usig limt or reduce the siae of ter esutl and puse
 * For paster perforame use rpaelles cans
 * Can use proejctionexpresssion _ FilterExpressions

 ### Idnexes
 * LSI : Local SEcondary Index , Alter range key for your table ,l coals to the has key
 * upt of five localsecodn indexeer per atable
 * the sort cekye consist of exactly one scala attribute
 * the attribute that you choose mus t be scalar string
 * these can be created only on table creation time

 * GSI : 
 * To speed up queeries on non-key attribute usse golbaos seconay index
 * GSI = partion key + optinoal sor key
 * the indes is a net table and we can project attirbute to it
 * must define RCU / WCU for this index

### DynamoDB - DAX
* Seamless cache for DynamodDB , no application reqiret
* write go through DAX to DynamoDB
* Micros Sedond latency for aches read and queres
* solves the hot key problelm
* 5 minutes tttl for ache by defaul
* uypdate 10 noes in the clisuter
* mutli az ( 3 nodes minimu recommended for production)
* secure
* DAX Cluster must be provisioned

### DynamoDB Streams
* Changes in DynamoDB , Create ,update and delete end in stream
* the stream can be ready by AWS lambda and react based on them
* can implemnt CORS 
* Stream has 24 hours of dta retetion
* configurable batch size
* dynamodb streams kisnis adapter , use the kcl lbirary to directly consume from streams, alternative to using aws labmda

### TTL
* automatically delete an item after an expiry data/time
* ttl is free, deletion do nto use wcu/rcu
* background task, helps reduce sotrage 
* deletes expired item with 48 hours of epxiration
### Dynaom DB Seccuiryt
* VPC Endopoints avaialbe to access DynamoDB
* IAM
* Encryption at rest usking KMVS
* Backup and REstore
* Global taables
* Amzone DMS cna be use to migration Dyanmdob
* you cna luancha  local dynadom for your deveolpme t purppse

### AWS ElastCache
* ElastAche to get managed Redis of MecacheD
* Cachehs are in-moemyr data base with realy higly perfomanre, low latency
* Redis OVeriew : in memoney k,v store, super low latenc, cache is peresetnt
* Users ssions ,leaderbod,ditriuted states, relive pressue on dtabase, pub.sub scapciytt of 
* MechadeD : in memy object store , cached doesn't suvive reboots ,

