Notebook for PDE

# coursera specialization : Data Engineering on Google Cloud Platform Specialization
-----------------------------------------------------------------------------
  - URL : https://www.coursera.org/specializations/gcp-data-machine-learning

## course 1 : Google Cloud Platform Big Data and Machine Learning Fundamentals

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


### GCP products for data processing
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
    + feature
      - can use via standard SQL
        query on BigQuery can be executed via API, web console or CLI
        you don`t have to create a instance
      - tables belongs to dataset, datasets belongs to a project
      + pros
        - can deal with the biggest structured data among GCP products
    + 3 ways to load data
      1. load data from disk : e.g. local machine, GCS, Cloud Datastore
      2. stream data : from Cloud Dataflow or etc.
      3. prepare data as a text file (e.g. .csv, .json or Google Sheet) on GCS and set up a federated data source (i.e. link to the file location)

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

* Cloud Datastore
  - persistent hashmap database to scale up to *terabytes*
    (in comparison, relational database (e.g. Cloud SQL) has limit of a few gigabytes)
    (really?) when writing, you write entire object. when reading, you can search by both the key of object or a property of object.
  + notes
    + feature
      - transactional operation supported (e.g. update some column of some row in database)
  + how to use
    once you write constructor for Datastore, you can save|load object to|from Cloud Datastore directly inside script like java. (e.g. `some_object.save().entity(xjin)` to save)

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

* Cloud DataProc
  - managed Hadoop ecosystem (e.g. : Hadoop, Pig, Hive, Spark etc.)
  + notes
    + benefits of DataProc except basic cloud benefits (e.g. scalability)
      - can store data on GCS, not on cluster
        (if you store data on cluster, you have to keep them up 24-7.)
        so, you need to make cluster up only while you do some colculation.
      - DataProc cluster and preemptible VM machine are beautiful pair
  + how to use
    0. in Cloud SLQ instance, allow access from the IP address of the machine which user owns (e.g. Cloud Shell)
    1. create DataProc instance
    2. authorize access from DataProc instance to Cloud SQL instance via `gcloud sql instances patch` command
    3. create DataProc job
      - you select job type (e.g. PySpark) and script (e.g. .py file on GCS) here

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

## course 2 : Leveraging Unstructured Data with Cloud Dataproc on Google Cloud Platform


## course 3 : Serverless Data Analysis with Google BigQuery and Cloud Dataflow


## course 4 : Serverless Machine Learning with Tensorflow on Google Cloud Platform


## course 5 : Building Resilient Streaming Systems on Google Cloud Platform


# other notes
-----------------------------------------------------------------------------
## resources
  - github of GCP : https://github.com/GoogleCloudPlatform/training-data-analyst
    - training_data_analyst/CPB100/lab2b : get earchquake data from web and store it
