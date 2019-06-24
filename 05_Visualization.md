# Visualization

## Amazon QuickSight
* Fast , easy, clouder powered busines sanylitc service
* allows all employees in an organation to
    * Build visualizations
    * perfomr ad-hoc analysis
    * quickly get buiness inserig from data
    * anytime, on any deivce
* Serverless
* quickSight Data Sources
    * Redshif
    * Aurora
    * athena
    * EC2 host databases
    * Files
        * Excel
        * CSV
        * log format
    * Data preparation allows limited ETL

### SPICE
    * Data sets are imported into SPICE
    * Each user gets 10GB of spice
    * Hhiglya vailabe durable
    * Scalae to hunders of thoundsad of users

### Use Cases:
    * Interactive ad-hoc epxloration / visualaiton of dta
    * dashboarda and KPI's
    * Stories
        * Guided tours thorugh specic view fo any analysis
        * Convery key points, throught process, evluation of an ahnaylis
    * Analyze / visual data from    
        * Log insS3
        * on prem database
        * AWS 
        * SaaS Application shuc as Selesforce
        * Any JDBC/ODB data source
### Anti Patterns
    * Higly fomratted canned reports
    * ETL
        * Use gluce instaled ,athought quick sighe cand do some transfomrations

### Security
    * MFA 
    * VPC Connectivity
        * Add quicksighs ip address range to your datbase security groups
    * Row Level Security
    * Private VPC
        * elastic network interface, asw direct connect
### User Maangemetn
    * Users define via IAM or email signup
    * Active Directory Intergation with Quicksighe enterpise edition
### Pricing
    * Annual subscription
        * Standard : $9 / user / month
        * Profesional : $18 / user / month
    * Extrace SPICE Capacity
        * $ 0.25 , $ 0.38 / GB / month
    * Mont to Month
        * $12 std, $24 /gb/month
    * Enterpirse
        * Ecnryption at rest
        * Nicrsoft AD intergration

### Visual Types
    * Autograph : Auto-graph will automatically try to select the most appropriate visualization type for the data you have selected.
    * Bar Charts
        * for comparison and distribution , histograms
    * Line grpahs
        * for changes over time
        * Line charts are appropriate for viewing trends in data over time.
    * Scatter plots , heat maps
        * For correlation
    * Pie Graphs , tree maps
        * for aggregation
    * Pivot Tables
        * for tablular data
    * Stories
    * Dashboard : A dashboard is a way to publish a screen of data to a larger audience.

### Alternative Visulation tools
    * Web-based visualation tools (Deployed to the public)
        * D3.js
        * Chart.js
        * Highchart.js
    * Business Intellijence Tools
        * Tableau
        * MicroStrategy

