Notes for PDE

# notes for coursera specialization : Data Engineering on Google Cloud Platform Specialization
-----------------------------------------------------------------------------
  - URL : https://www.coursera.org/specializations/gcp-data-machine-learning

## course 1 : Google Cloud Platform Big Data and Machine Learning Fundamentals (1 week)
summary : introduction to GCP products for data engineering

* 3 stages of the journey to the cloud
  1. simply change where to compute (do sampe calculation as on-premise)
    - e.g. : Cloud SQL
  2. improve scalability and reliability with managed services
    - e.g. : Cloud DataFlow
  3. change how you compute
    - e.g. : Google Cloud Vision API

* stepping stone to transformation to GCP
  - some company find difficulties to transform to GCP completely, so stepping stones are needed.
  - e.g. : compute engine -> container engine -> app engine


## course 2 : Leveraging Unstructured Data with Cloud Dataproc on Google Cloud Platform
summary : guidance of Cloud Dataproc and Cloud Dataflow?

* dealing with unstructured data is big issue
  - as much as 90% of data is unstructured data (here, "unstructured" doesn`t mean "not stored in RDB", but means "collected not for the same purpose as one that we want to use it for"
  - useful tools
    - Cloud Dataproc with data pipeline and retful ML APIs

* 4 stages of the cloud-shift of Hadoop
  1. on premise original Hadoop
  2. on premise vendor Hadoop
  3. bdutil (free OSS toolkit) on GCP
    - created, deployed and connected to GCP by Google
  4. Cluod Dataproc
    - in addition to above features, fully maintained by Google


## course 3 : Serverless Data Analysis with Google BigQuery and Cloud Dataflow
summary : guide to BigQuery and Cloud Dataflow

## course 4 : Serverless Machine Learning with Tensorflow on Google Cloud Platform
## course 5 : Building Resilient Streaming Systems on Google Cloud Platform



# notes for GCP products for data processing
* comparison among storage solutions
  (order of stepping stones from on-premise to full-managed-cloud)
  1. Cloud Storage : storage for BLOB (Binary Large OBject)
  2. Cloud SQL : No-ops (No operations) SQL database on the cloud
  3. Cloud Datastore : persisitent hashmap for Structured data from AppEngine apps
  4. Cloud Bigtable : No-ops flattend key-value store accesible via HBase API for with high-throughput and scalability
  5. BigQuery : fully managed data warehouse accessible with SQL
* App Engine

* Cloud Bigtable
  - key-value store to scale up to *terabytes*
    (in comparison, relational database (e.b. Cloud SQL) has limit of a few gigabytes)
  + notes
    + feature
      - good for flatten data (not hierarchical data)
        - you can create column family by add prefix (e.g. "FIRST_CATEGORY:SECOND_CATEGORY:value") to column
      - serach data only by key
        - in stead of searching by a property of row, you should make key contain such informations
        - you should design key not to make hot spot (a bucket with too much data) by combining key with random number
      - to update a row, add a new row instead
        (the old row remains, but it`s ok because rows are searched from latest)
      - can use it via HBase API
        (Apache HBase : a open source, distributed, scalable, big data table (wide column store))
      + pros
        - high-throughput write
      + cons : what is given up to get more scalability than Cloud SQL
        - transactional operation is NOT supported
        - updating of just single field of object is NOT supported
        - key-value store, so you cannot search data by attribute (you can on Cloud Datastore)

* BigQuery
  - fully-managed data warehouse to run ad-hoc SQL queries on *petabytes* of data
  + notes
    + pros and cons
      + pros
        - can deal with the biggest structured data among GCP products
        - can deal with data of complex structure (e.g. json)
        - cost for data storage is inexpensive : almost same as Cloud Storage
          (the discount rate of long use is almost same, too.)
      + cons
    + basic features
      - can use via standard SQL (SQL 2011)
        query on BigQuery can be executed via API, web console or CLI
        you don`t have to create a instance
      - tables belongs to dataset, datasets belongs to a project
      - good for near-real time analysis (delay : seconds)
        - for true real time analysis (delay : microseconds), Cloud SQL or something like Spanner is good.
      - accepted text file : CSV, JSON, AVRO, or Cloud Datastore backups
    + fee
      1. for storae
      2. for query (two plan : "pay for use" (recommended) and "flat plan")
    + ways to upload data
      1. via BigQuery web UI (which is available on Cloud Console)
        1) create a dataset and a new table in it via BigQuery UI.
           When creating, select "Created table from : Upload" and specify the file to upload.
      2. via `bq` (BigQuery CLI)
        1) run `bq` command with proper options on Cloud Shell
      #1. load data from disk : e.g. local machine, GCS, Cloud Datastore
      #2. stream data : from Cloud Dataflow or etc.
      #3. prepare data as a text file (e.g. .csv, .json or Google Sheet) on GCS and set up a federated data source (i.e. link to the file location)
    + ways to export data
      1. receive to a bucket of Cloud Storage
    + grammer of SQL on BigQuery
      + ARRAY, STRUCT : can be used for grouping data
      + normalize v.s. denormalize
        normalizing data make data more complact, but denormalizing data make query faster.
        you can use "nested repeated fields" as a middle option of them : make query faster while reserving relationalism of the data
        (e.g. you can nest fields `foo.bar` and `foo.baz` under `foo`. 
              nested field `foo` can be unnested by `UNNEST(foo)` (similar to EXPLODE()) in FROM clause or `foo[OFFSET(0)].bar` in SELECT clause)
      + UDF
        - JavaScript and SQL UDFs are supported with some limitations.
          - amount of data to process
          - number of UDFs defined and used
        - for faster query, use `APPROX_***` UDFs
          (e.g. APPROX_COUNT() is faster than COUNT(), but not precise)
      + partition
        partitions is efficient especially when dealing with time-based data
        - by declare `time_partitioning_type` or `time_partitioning_expiration` on creating table,
          BigQuery automatically store data in collect partition
    + function of BigQuery
      + performance monitoring
        1. per-query performance : can be checked by explain plans
        2. project-level monitoring : can be checked by Google Stackdriver

* Cloud Dataflow
  - data pipeline : Apache Beam on GCP
  + notes
    + pros and cons
      + pros
      + cons
  + basic features
    - each step (called 'transform') of pipeline is automatically-scalable
    - adaptive to both batch process and stream process
    - pipeline can be written in both Java and Python
    - can ingest data from/output data to GCS, BigQuery, Pub/Sub and a file
  + additional features
    + MapReduce on Cloud Dataflow
    + side input
  + usage
    1. write code to create a graph of pipeline
      e.g.
        p = beam.Pipeline()
        (p | <some_process_to_apply> | ... )
    2. write code to run the graph
      e.g.
        p.run()
    3. run the code
      (a) run locally
      (b) run on GCP
    + terms
      - PCollection : parallel collection(取得) of data
                      represent data in pipeline
                      (?) not in-memory collection, but can be outbound

* Cloud Datalab
  - Jupyter notebook with authenticated access to GCP products
  + feature
    - run on GCE, so easy to scale up
    - can use git in GUI from "ungit" buttom
    + use case
      - can develop big data algorithms *interactively* in Python
  + how to use
    1. run Datalab from CLI like CloudShell or local CLI
    2. access to a specific port of the GCE machine where a Datalab runs
    * in combination of Datalab and BigQuery, you can interectively run queires on massive dataset and get result in a pandas.DataFrame

* Cloud DataProc
  - managed Hadoop ecosystem (e.g. : Hadoop, Pig, Hive, Spark etc.)
  + notes
    - suitable for terabytes or petabytes of data
      (for gigabytes or terabytes of data, running my own Hadoop cluster is more efficient than Cloud Dataproc)
    + benefits of DataProc except basic cloud benefits (e.g. scalability)
      - can store data on GCS, not on cluster
        (if you store data on cluster, you have to keep them up 24-7.)
        so, you need to make cluster up only while you do some colculation.
      - DataProc cluster and preemptible VM machine are beautiful pair
    + basics about Hadoop
      - Hadoop : distributed processing system
      - HDFS : distributed file systems
  + how to use (way 1, simplest way from course 1)
    0. in Cloud SLQ instance, allow access from the IP address of the machine which user owns (e.g. Cloud Shell)
    1. create DataProc instance
    2. authorize access from DataProc instance to Cloud SQL instance via `gcloud sql instances patch` command
    3. create and run a DataProc job from GCP console
      - you select job type (e.g. PySpark) and script (e.g. .py file on GCS) here
  + how to use (way 2, detailed way from course 2)
    1. create and connect Cloud Dataproc machienes from CLI (gcloud command from Cloud Shell or GCE instance) or REST API
      - GCE instance is better than Cloud Shell because only the former has SLA.
    2. create a GCS bucket for staging Dataproc
      - you can use a GCS bucket to stage (1) Dataproc application or data files, (2) Dataproc initialization scripts and output
      - by creating initializing-scripts for Dataproc clusters, you can automate cluster creation with Cron, Jenkins, etc.
        (e.g. : add a node when PySpark job file is added.)
    3. setting fire wall setting of Dataproc nodes from GCP console to connect to Dataproc cluster from your browser
    4. now, you can use hadoop like ...
      1. access to hadoop GUI such as below from your local browser
        (how : access to <external_IP_of_hadoop_master_node>:<port> from the browser)
        - applicatons interface : you can see all hadoop jobs running
        - administration interface : you can see informations, files and logs of hadoop nodes
  + configurations on creating a instance
    - zone : if data is on GCS, zone should be matched
    - master node : setting up clusters
      1. single node : use only single Hadoop node for experimentation
      2. standard : use 1 master node
      3. high availability : use 3 master node (tolerant of instance-loss)
    - machine type : select among machines with various number of CPUs and memories
      - if you want additional memory, you can select "highmem" machine types
    - preemptible worker nodes
      - you can use it only for processing, not for hdfs (reason : it disappers anytime)
      - maximum 50/50 ratio between persistent and preemptible machine is recommended to reduce the likelihood of job failure


* Cloud Datastore
  - persistent hashmap database to scale up to *terabytes*
    (in comparison, relational database (e.g. Cloud SQL) has limit of a few gigabytes)
    (really?) when writing, you write entire object. when reading, you can search by both the key of object or a property of object.
  + notes
    + feature
      - transactional operation supported (e.g. update some column of some row in database)
  + how to use
    once you write constructor for Datastore, you can (save|load) object (to|from) Cloud Datastore directly inside script like java. (e.g. `some_object.save().entity(xjin)` to save)

* Compute Engine
  + notes
      - if you use a machine for longer than 20% of a month, you get discount. (In return for it, there is no cheep machine-class for long use.)
      - preemptible machine
        - a machine you get a great (e.g. 20%) discount in return for letting go off when you don`t essencially need it (e.g. node in hadoop cluster)
    + how to use
      - 1. create VM instance
      - 2. ssh access to the instance

* Cloud Storage
  + notes
    - can store any data in any format
    - bucket : address of data. bucket name must be unique.
    - region and zone
      - use closest region to reduce latency
      - use multiple regions to provide global access
    - can publish a file to all uses directly from GCS
  + how to interect with GCS
    - gcloud and gsutil (CLI)
    - REST API (Application)
  + command
    - gsutil cp <file> <path> : upload a file to GCS
      - <path> : e.g. gs://<bucket-name>/earthquakes/
    - gsutil acl ch : e.g., publish a on GCS to all users
    - gsutil mb : move bucket
    - gsutil rb : remove bucket
    - gsutil rsync : sync file in GCS to local one

* Cloud Lancher
  - can lanch VM instanve with some packages preinstalled (e.g. wordpress)

* Cloud SQL
  - managed MySQL or postgreSQL database
  + notes
    - adavntages of Cloud SQL over VM instance with SQL gotten from Cloud Lancher
      - flexible pricing
      - Google manges backup
      - Google secures the database
      - (can connect from anywhere)
      - (fast connections to GCS)
    - Relational database is best choce for several 100 megabytes structure data, but not for terabytes of data (become too slow)
  + how to use
    0. create a CLoud SQL instance
    1. import "create talble" .sql on GCS to create empty tables
    2. import .csv file to fullfill the empty table with data
    3. execute SQL query on Cloud SQL via mysql CLI using cloud-sql-proxy as support

* Cloud ML Engine
  - no-ops machine learning application

* Cloud Pub/Sub

* DataFlow

* ML APIs
  + how to use
    1. search it from APIs&Seavices/Library, and enable it.
    2. get credential to use it from APIs&Searvices/Credentials.

* TensorFlow
  - open-source Python API of C++ program for running machine learnings on CPU and GPU
  + notes
    -

* Spark
  + Spark MLlib : distributed machine learning library
  + 2 way of use
    1. `pyspark` : start PySpark shell
    2. `spark-submit <file_of_spark_job_written_in_Python>
  + how to use
    0. enter PySpark shell by executing `pyspark`
    1. create a Spark RDD (Resilient Distributed Dataset : basic abstraction over data in Spark) from testfile
    2. execute any processing on the Spark RDD object using Python and PySpark
      - parallel processing by MapReduce is available

* Pig
  - Pig provides SQL primitives similar to Hive, but in a more flexible scripting language format. Pig can also deal with semi-structured data, such as data having partial schemas, or for which the schema is not yet known. For this reason it is sometimes used for Extract Transform Load (ETL). It generates Java MapReduce jobs. Pig is not designed to deal with unstructured data.

* networkings
  + VPC network (location : GCP Console > VPC network)
    - you can set up firewall to permit|deny access from outside to GCP storages and instances
      (location : VPC network > Firewall rules > Create Firewall Rule)


# other notes
-----------------------------------------------------------------------------
## useful resources to learn
  - github of GCP : https://github.com/GoogleCloudPlatform/training-data-analyst
    - training_data_analyst/CPB100/lab2b : get earchquake data from web and store it

## technical terms
+ 2 way of dealing with large amout of data
  1. scaling up, vertical scaling : old way. use a bigger single machine
  2. scaling out, horizontal scaling : new way. use distributed parallel processing
    - difficult, but efficient
+ 2 kind of programming
  1. imperative programming : yow tell the system what to do and how to do (e.g. C++)
  2. declarative programming : you tell the program what you want, and the program figures out how to implement it (e.g. Spark)

## trivial notes
- Githubでホストしている.mdファイルをconfluenceに貼り付けられないかと思ったが、有料のmarkdownレンダリングマクロ(の1機能として.mdファイルをurlで指定できる)が必要なようだ

## history
- 20190511 WIP on course 3
- 20190330 finished course 2
- 20190214 finished sourse 1


