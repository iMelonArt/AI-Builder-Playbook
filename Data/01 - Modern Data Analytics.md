# Modern Data Analytics



# Data warehouse vs Data Lakehouse & Data Lake

A **data warehouse** is a structured repository optimized for fast querying and analysis of organized data, primarily used for business intelligence. A data warehouse is an enterprise system used for the analysis and reporting of structured and semi-structured data from multiple sources, such as point-of-sale transactions, marketing automation, customer relationship management, and more. 

A **data lake** stores vast amounts of raw, unstructured data from various sources, providing flexibility for future data processing and analysis.

A **data lakehouse** combines elements of both data warehouses and data lakes, offering structured data management with the flexibility of handling unstructured data for comprehensive analytics. For instance, a lakehouse such as Microsoft synapse () is a collection of files, folders, and tables that represent a database over a data lake used by the Apache Spark engine and SQL engine for big data processing.

# Data mart
A data mart is a structure/access pattern specific to data warehouse environments, used to retrieve client-facing data. The data mart is a subset of the data warehouse and is usually oriented to a specific business line or team.

# One Lake

OneLake is a single, unified, logical data lake for your whole organization. 

## OLTP vs OLAP

OLAP (Online Analytical Processing) is designed for complex, read-heavy analytical queries on large volumes of historical data using denormalized schemas, while OLTP (Online Transaction Processing) focuses on managing real-time transactional data with simple, write-heavy operations using normalized schemas for fast query processing and minimal data redundancy.

## HTAP

Before [HTAP](https://www.snowflake.com/guides/htap-hybrid-transactional-and-analytical-processing/), if an organization needed to run both OLAP and OLTP workloads, the configuration would require two separate databases, with each one optimized for the data workloads it was expected to perform. OLTP databases were needed for transactional processing, designed for executing single-record operations at scale with accuracy and speed, while OLAP databases were needed for multi-dimensional analytical queries and processing massive amounts of complex data. Making data from OLTP systems available for analysis in OLAP systems required data to be transferred from one system to the other using complex and time-consuming extract, transform, load (ETL) pipelines. 

Although some organizations use an operational data store (ODS) as a way to bridge these two systems, this solution can be costly and complex to implement and fails to fully address the lag time in making transactional data available for analytical queries. ODS is more of a stop-gap measure that introduces an additional system that needs to be managed. HTAP was initially developed to support both OLAP and OLTP without creating additional layers of complexity.

BENEFITS OF HTAP
Data Cloud for Dummies
Cloud Data Lake for Dummies
Unifying transactional and analytical data unlocks many new opportunities to extract value from data stores. Although running parallel OLTP and OLAP systems is still common practice for many organizations, the advantages of uniting these two systems has made HTAP an increasingly popular choice. 

# Big Data Analytics 

Hadoop
Spark





## Big Data Analytics Achitecture - Lambda  vs Kappa 

### Lambda achitecture



### Kappa architecture
The Kappa Architecture is considered a simpler alternative to the Lambda Architecture as it uses the same technology stack to handle both real-time stream processing and historical batch processing. 

### Choosing between lambda and kappa architectures
Both architectures handle real-time and historical analytics in a single environment. However, one major benefit of the Kappa Architecture over the Lambda Architecture is that it enables you to build your streaming and batch processing system on a single technology. There is no separate technology to handle the batch processing, as is suggested by the Lambda Architecture.

## Product Showcases

The following is a breif introduction for different products from major providers.

### Microsoft Fabric
Azure Data Platform 


Big data analytics from HDInsights to 


Microsoft Fabric is an end-to-end analytics and data architecture platform designed for enterprises that require a unified solution,  for storing, managing, and analyzing structured and unstructured data in a single location. It encompasses data movement, processing, ingestion, transformation, real-time event routing, and report building. It offers a comprehensive suite of services including Data Engineering, Data Factory, Data Science, Real-Time Analytics, Data Warehouse, and Databases.

One Lake - Store
Data Management
Data Security ( access, role control, permssions)
Data Discovery
Data governance


### Google Big Query

BigQuery is a fully managed enterprise data warehouse that helps you manage and analyze your data with built-in features like machine learning, geospatial analysis, and business intelligence. BigQuery's serverless architecture lets you use SQL queries to answer your organization's biggest questions with zero infrastructure management. Federated queries let you read data from external sources while streaming supports continuous data updates. 

SQL-like syntax, Petabyte insights. 


### AWS Redshift


Amazon RedShift is provisioned on [clusters and nodes](https://docs.aws.amazon.com/redshift/latest/mgmt/overview.html) and it also supports [Serverless computing](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-considerations.html) for data analytics.

The maximum number of characters for a table name is 127. The maximum number of columns you can define in a single table is 1,600. The maximum number of SORTKEY columns you can define in a single table is 400.


### DataBricks

The key components and features of the databricks lakehouse platform include:

- Delta lake : 
An open-source storage layer that ensures data quality, enables ACID transactions and provides scalable and performant query capabilities for large datasets. It supports popular data formats like Parquet, Avro, and JSON, and integrates with Apache Spark for processing.

- Databricks runtime

A managed and optimized version of Apache Spark that provides better performance, reliability, and ease of use. It supports various data processing tasks, including ETL, machine learning, and graph processing.

- Databricks workspace


- Databricks machine learning


- Databricks SQL analytics



Databricks supports both clusters & nodes computing as well as [serverless computing](https://docs.databricks.com/en/workflows/jobs/run-serverless-jobs.html#:~:text=Serverless%20compute%20is%20supported%20with,compute%20for%20all%20job%20tasks.) for spark jobs for workflows.


Focus on the notebooks ( data scientists, data engineers)




### Snowflake
Cloud-based Data warehouse, separation of compute and storage. in 2016, data storage (Amazon S3 and S3-Compatible solutions ) to store table data and query results, virtual warehouses ( handles the query execution within elastic clusters of vms ), engine ( the brain of the system, queries, transactions, meta data, data schema, access control), micro-partitions


242 files, 12 secs (188ms) and 20M records
large warehouse

Describe their computing as T-Shirt sizes, credit usage per hour, X-Small, Small, Medium, Large and all the way to 5X Large and 6X-Large.


### Dremio

The [Dremio Unified Lakehouse Platform](https://www.dremio.com/platform) lets you connect, govern, and analyze all of your data, both in the cloud and on-premises.

## Old School warehousing

### Teradata 
[Teradata](https://www.teradata.com/) is the most popular type of Rational Database Management System. It is a processing system that is widely used for developing large-scale data warehouse applications. Teradata will develop large-scale data warehouse applications with the help of Parallelism.

### Oracle Exadata
The [Oracle Exadata Database](https://www.oracle.com/engineered-systems/exadata/) Machine is a computing platform optimized for running Oracle Databases. Exadata is a combined hardware and software platform that includes scale-out x86-64 computer and storage servers, RoCE networking, RDMA-addressable memory acceleration, NVMe flash, and specialized software

### IBM Netezza
[IBM Netezza Performance Server](https://www.ibm.com/products/netezza) is a cloud-native enterprise data warehouse designed to operationalize deep analytics, business intelligence and AI/maching-learning (ML) workloads by making data unified, accessible and scalable, anywhere. With new support for open formats such as Parquet and Apache Iceberg, plus native integration with your data lake.

### SAP (HANA, BW)


### Cloudera/Hortonworks


### Apache Hive

The Apache Hive is a distributed, fault-tolerant data warehouse system that enables analytics at a massive scale and facilitates reading, writing, and managing petabytes of data residing in distributed storage using SQL.



## Industry Showcases

Banking & Insurance ( Financial )



Retail



Healthcare 





