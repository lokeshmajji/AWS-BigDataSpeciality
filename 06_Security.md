# Security

## Encryption in Flight
## Encryption at Rest

## S3 Encryption for Objects
    * SSE - S3
        * Encryption keys are handled and managed by AWS S3
        * AES 256 
        * Must set header : "x-amz-server-side-encryption":"AES256"
        * S3 will create managed data key and encrypt the object and stored in the bucket
    * SSE - KMS
        * Keys handled & managed by KMS
        * User control + Audit trail
        * Ojbect is encrypted at server side
        * Must set header : "x-amz-server-side-encryption":"aws:kms"
    * SSE - C
        * SSE-C : server side encrption usind data keys full managed by the customer outside of aWS
        * S3 doesnt store ethe encryption key you provide
        * HTTPS must be used
        * Encryption keey must provide in HTTP headers for revery HTTP request made
        * Server will encrypt the object and the data key p[rovide by the client and deos the encryption
    * CSE
        * Clinet libray such as the amaxon  s3 encryption client
        * client mus tencryp data themeserverl before seindogn to S3
        * client must decrypt data themeserverl before seindogn to S3
        * custome fully manages the keys and data
    * HTTP/ HTTPS
    * HTTPs is required for CSE
    * Encrptpiont is calles SSE/TLS
### KMS
* The value in KMS is that the CMK used to encrpyt data can never e retieved by the user , the CMK can be rotated for extra security
* Nevers store your secreates in palintext
* Encrypted secreated can be stored in the code/environemnt vaiable
* KMS can only help in ecnrypted up to 4KB of data per call
* If data > 4 KB use enveopoe encrypted
* API Encrypt 
* Require Migration thorugh Snaphsot / Backlup
    * EBS
    * RED
    * ElastiCache
    * EFS
* S3 can directly use the KMS

### Cloud HSM
    * KMS => Aws managed the sofware for encryption
    * CloudeHASM => AWS Provision encryption hardeared
    * Dedicate Hardware 
    * Your managey our own encprtoin keys netirely
    * The cloudeHSM haresare device is tamper resitancacte
    * FIPS 140-2 Level 3 complinace
    * CloudeHSM cluster are spread acess mult AZ
    * Supprots both sysmmetir and assyment entryuptiojn (SSL/TLS)
    * Must use the Cloude HSM Software

### Kinesis Security
* Kinesis Data Streams
        * SSL Endoput using the thtp protoco lto the encryption in flight
        * AWS KMS provides server sideencryption
        * for cse your must use youro nw encryption 
        * supported interface vpc edon ponts / privatel ink access privates
        * KCL - must get reade /wriea cess to ydmanodb talbe
* Kinesid Dat Fireshose:
        * Attach IAM roels so it can deliver to S3/ES/REshdiht/Splunk
        * can encryp the delivery streem with KMS
        * Supprted interace VPON Endpoints / Pvivate link
* Kinesis Data Anlytics
        * Attache IAM reols s oit can rea fro mdinkes data stream and reference source and wirt to an output desition ex: kinesis data firesose

### SQS Security
* Encypriotn in flihg using thtps endpoint
* SSE kms
* IAM plicy must allow usage fo sqs
* sql queue caccess plicy

### AWS IOT 
    * IoT Polices
        * attahced to X.509 certiface or congitoin identied
        * able to revoe an ydevie at any time
        * IOT polcie are josn documents
        * can be attached to gorupes insera fof ainviudal things
    * IAM Polices
        * Attache to uses group or roels
            * used for ocntorlling IOT AWS APIs
    * Attache role s to urles osthat thye can prefor Iot
### S3
    * IAM Polices
    * s3 buket polcies
    * ACLS
    * encyrptio nin flihg
    * encryp at rest
    * VErison 
    * MFS
    * CORS 
    * VPC endpint is prohvided throug gatwa
    * Glacier - value lokce police to prvent delteds 
### Dyanomddob
    * HTTPS
    * KMS encrypt for base table and secondar indees
    * only for new tables
    * to mgirated inecry data le create new table and copy the dtaa
    * encyrp cannot be disble once enabled
    * acces to tables/ap/das using IAM
    * Dyamobd stream do not support encyrpt
    \* VPC endopint is provide through a ageway

### RDS
* VPC provies ntework istoatin
* SGroup control network acess to DB instances
* KMS provide encryption at rest
* SSL provide encyrpitno in flihgt
* IMA plice proived proteion for the REDS API
* IAM Authenticaiotn is suported by PostgreGSQL and Mysql
* Must manage user permission with the e database itselft
* MYSQL SErve and ORale spport TDE (Transpernet Data Encryption)

### Aurora
* VPC
* SGroup control network acess to DB instances
* KMS provide encryption at rest
* SSL provide encyrpitno in flihgt
* IAM Authenticaiotn is suported by PostgreGSQL and Mysql
* Must manage user permission with the e database itselft
* MYSQL SErve and ORale spport TDE (Transpernet Data Encryption)

### Lambda
* IAM roles attached to each lambed afunction
* Sources
* Tragerts
* KMS Encryption for secretes
* SSM parameter stored for ocnfiugrations
* CloudWatch Logs
* Deploy in VPC to access private resources

### Glude
* IAM
* Confgule Glue to acess JDBS thourhg SSL
* Cataglog Encrypte by KMS
* Connectio0n passwords : encrypted by kms
* Data writen by AWS Glues
    * S3 encryption mode
    * Cloudwath encyrpton mode
    * job bookmoart encryption mode

### EMR - Really Important
*  EC2 Key pair for SSH Creds
* Attach IMA Roels to EC2 instance
    * Acces s to S3
    * EMRFS to reuest to S3
    * DyamodDb scnae thorugh Hive
* EC2 Security groups
    * one for master node
    * Another one for cluster node (core node or task node)
* Encryp data at rest
    * EBS encryption
    * Open source HDFS Encryptoihn
    * LUKS + EMRFS for S3

* In tranist encryption : node to nde communcaiton ,emrfs, tls
* Date is encryptie befor uploadign to S3
* Kerverose authentication from AD
* Paache Ranger : Centraized Aurhtroization (RBAD) - setup on extyernal EC2

### Elastic Serach
    * Amzon VPC provide netowrk istaloatin
    * Elastic seach plicy tom ange security further
    * Data security by encrypint data a rest using kms
    * ernyptin in trasi using SSL
    * IAM or codnit base authatenciation
    * Amzon co ndition - Active Deirctoy + SQML

### Redshift (Important)
    * VPC provide ntwork sistoation
    * cluster secuiryt groups
    * encryuti in fligh using th djbc seriver renabled with ssl
    * encuyrpti at restu sign kms or a hsm device
    * supprots S3 SSE using defualm nage kay
    * USAI AM reosl for redshift
    * To acces s other AWS resourse
    * Must be referencei nthe copy or unload command, alternatily paste access key and esecretkey creds

### Athena
    * IAM polices 
    * Data in S3
    * Encryption of data according to S3
    * Encryptoin in transi using TLS Between Athena and S3 and JDBS
    * Fine graing using the AWS gloue Cataglog

### Secuiryt
    * Standaed eition
        * IAM users
        * EMail based account
    * Entprise Edition
        * AD
        * Fdeeragg Log
        * MFA
        * Encyrpt at rest and in SPCIE
    * Row leve serucity to contro which users cna ese wwhich rows

### AWS STS
    * Allows to grantl imited and tomprary access  to AWS Resource
    * Tokes in vlaid for up to one houre
    * Corss account access
        * Allows user from one AWS account acess sresoure ina ntoher
    * Federation (Active Directoyr)
        * provide a non-AWS use with temproray WAS access by linkng users active directory credntials
        * uses SAML
        * Allows single sigon on which enables user to loign ti to AWS consle withou assing IAM credetniaosl
    * Federatio with thirdp arty prvoide / congition
        * used in we adn mobiel apps
        * makes use fo face/amaozne.google
* Cross Account Access
    * Deinfe IAM Role for anothe account to acess
        * Deinfe which accoutn can acess this IUMA Role
        * Use AWS STS to retive cred sand imprson the ima role you th acces 
        * temporary crede can be valide to 15 to 60 ,oms
* Idnetify Fderation
    * Federation let use outside of AWS to assume tomeprat role to accesing aws reosurces 
    * these suer assume dientiyt provided access role
    * 3rd party authentication
        * LDAP
        * AD SAML
        * SSO
        * Open ID
        * Cognito
    * SAML Federation for Entreprises
        * TO intergarte AD / ADFS with ADWS
        * Provides access to AWS conoel or CLI
        * No need to create an IAM uiser for each for your employees
    * Custome Identity Broker Applicaiton for Enterpises
        * Use only if identity provider is not comapitb with SQML 2.0
        * The dientiyt borker must detime the appropriateio IAM policy
    * AWS Congitos
        * Goald : Provide direct access to AWS resource from the client sidet
        * Login to fderated identy provide
        * Get temoraty aWS credetiona back from the federated identiy pool
        * Thesse credetial come wiht a predfine ima polic stating hteri permissions
    * provide access to wire tos3 buket using facebook login

### Policies - leverage AWS Variables
    * ${aws:username}
    * ${aws:principalType}
    * ${aws:PrintipclaTag/Department}
    * ${aws:FederatedProvider}
    * ${www.amaozne.com:user_ID}

### AWS CloudeTrail
* Provide goernacen, compilacne and audit for you AWS Account
* Enabled by default
* Get an yhist of event api callde made iwthy our AWS account by
    * Console
    * SDK
    * CLI
* CloudeTrail show the past 90 day of actity
* the defaul UI only show createm,modyf or delte events
* Get a detail lis of all the envets your hose
* abilit tyo store tehse enerts sins S3 for futerh anlaysis
* canb e geriona specif for globa
* encryptiend us sse s3
* control using IAM rules

### VPC Endpoints
* Endpoint allows you to connec to AWS Service suing a private netowrk isntead of the  publlic wWwn etowrk
* they scal horazatally and are redunatna
* they remove th need fo IGW,NAT etc to access AWS SErvercie
* Gateway : providions a tarege and muste in a route table only S3 and DynamodDB are gateways
* Interace : others services prvoidion an ENI