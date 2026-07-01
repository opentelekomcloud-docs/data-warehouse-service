:original_name: dws_01_0091.html

.. _dws_01_0091:

Functions
=========

This section describes the main functions of DWS.

Creating a DWS Cluster
----------------------

To use data warehouse services on the cloud, create a DWS cluster first. You can select product and node specifications to quickly create a cluster. For details, see "Creating a DWS Cluster" in *Data Warehouse Service User Guide*.

DWS Cluster Management
----------------------

DWS allows you to manage clusters in either of the following ways:

-  **Console**

   Use the management console to access DWS clusters. When you have registered an account, log in to the management console and choose **Data Warehouse Service**.

   For more information about cluster management, see "DWS Cluster Management" in the *Data Warehouse Service User Guide*.

-  **REST APIs**

   Use REST APIs provided by DWS to manage clusters. In addition, if you need to integrate DWS into a third-party system for secondary development, use APIs to access the service.

   For details, see *Data Warehouse Service API Reference*.

Connecting to a DWS Cluster Database
------------------------------------

After creating a cluster, you can connect to its database by using the SQL editor or third-party drives such as JDBC and ODBC. For details, see "Connecting to a DWS Cluster" in *Data Warehouse Service User Guide*.

To connect to a DWS cluster, perform the following steps:

#. Obtain the connection address of the cluster.
#. (Optional) Use the SSL encryption connection mode.
#. Connect to the cluster and access the database in the cluster. You can choose any of the following methods to connect to a cluster:

   -  Use the SQL client tool to connect to a cluster.

      -  Use the Linux gsql client to connect to a cluster.
      -  Use the Windows gsql client to connect to a cluster.
      -  Use Data Studio to connect to a cluster.

   -  Use the JDBC, ODBC, psycopg2, or PyGreSQL third-party driver to connect to a cluster.

Multiple Database Tools
-----------------------

DWS provides the following self-developed tools. You can download the tool packages on the DWS console. For how to use the tools, see the *Data Warehouse Service (DWS) Tool Guide*.

-  **SQL editor**

   The DWS SQL editor provides one-stop data development, ingestion, and processing functions. With the editor, you can connect to a cluster database from the DWS console to edit and execute SQL statements.

-  **gsql**

   gsql is a CLI SQL client tool running on the Linux OS. It helps connect to, operate, and maintain the database in a DWS cluster.

-  **Data Studio**

   Data Studio is a SQL client tool with a Graphical User Interface (GUI) that runs on Windows. It is utilized to connect to databases in a DWS cluster, manage database objects, edit, run, and debug SQL scripts, and view execution plans.

-  **GDS**

   GDS is a data service tool offered by DWS that utilizes the foreign table mechanism to achieve fast data import and export.

   The GDS tool package needs to be installed on the server where the data source file is located. This server is called the data server or the GDS server.

-  **gs_dump and gs_dumpall**

   **gs_dump** exports a single database or its objects. **gs_dumpall** exports all databases or global objects in a cluster.

   To migrate database information, you can use a tool to import the exported metadata to a target database.

-  **gs_restore**

   During database migration, you can export files using **gs_dump tool** and import them to DWS by using **gs_restore**. In this way, metadata, such as table definitions and database object definitions, can be imported.

Enterprise-Class Data Warehouses
--------------------------------

After a DWS cluster is created, you can use the SQL client to connect to the cluster and perform operations such as creating a database, managing the database, importing and exporting data, and querying data. For details about database operations, see *Data Warehouse Service Developer Guide*.

DWS provides high-performance databases that can handle petabytes of data, with the following features:

-  MPP computing framework, hybrid row-column storage, and vectorized execution, enabling response to billion-level data correlation analysis within seconds
-  Optimized in-memory computing based on Hash Join of Bloom Filter, improving the performance by 2 to 10 times
-  Supports the symmetrically distributed, active-active multi-node cluster architecture, ensuring no SPOFs.

-  Optimized communication between large-scale clusters based on telecommunication technologies, improving data transmission efficiency between compute nodes
-  Cost-based intelligent optimizers, helping generate the optimal plan based on the cluster scale and data volume to improve execution efficiency

DWS Development and Design Specifications
-----------------------------------------

Before building a database, developers specify the design specifications for database modeling and database application development. Modeling compliant with these specifications fits the distributed processing architecture of DWS and provides efficient SQL code. For details, see "DWS Database Object Naming Rules" in *Data Warehouse Service Developer Guide*.

Database User Permission Management
-----------------------------------

DWS allows you to manage database users on the console. You can create, delete, and update database users and manage their permissions on the console. This feature enables you to manage database users and user permissions on the console without logging in to databases. For details, see "Creating a DWS Database and User" in *Data Warehouse Service User Guide*.

Diverse Data Import Modes
-------------------------

DWS allows you to import data from diverse sources using various methods. For details, see "Data Migration to DWS" in the *Data Warehouse Service Developer Guide*.

-  Importing data from OBS in parallel: You can import data in TXT, CSV, ORC, or CarbonData format from OBS to DWS for query, and can remotely read data from OBS. This import method is recommended by DWS.
-  Using GDS to import data from a remote server: Use the GDS tool provided by DWS to import data from the remote server to DWS in parallel. Multiple DNs are used for the import. This method is efficient and suitable for importing a large amount of data to the database.
-  Importing data from MRS to a DWS cluster: You can configure a DWS cluster to connect to an MRS cluster, and read data from the HDFS of MRS to DWS.
-  Using the gsql meta-command **\\COPY** to import data: The gsql tool of DWS provides the **\\copy** meta-command to import data.
-  Using COPY FROM STDIN to import data: This method is applicable to low-concurrency scenarios where a small volume of data is to be imported.
-  Using **gs_dump** and **gs_dumpall** to export metadata: Export required database objects or related information using the two tools provided by DWS.
-  Using **gs_restore** to import data: During database migration, you can export files using **gs_dump** and import them to DWS using **gs_restore**. In this way, metadata, such as table definitions and database object definitions, can be imported. The following definitions need to be imported:

   -  All database objects
   -  A single database object
   -  A single schema
   -  Definition of each table

-  Other: You can also use the **INSERT** statement, **COPY FROM STDIN**, gsql meta-commands, and third-party ETL tools to import data.

Exporting Data
--------------

DWS allows you to export data in three ways. For details, see "Exporting Data" in *Data Warehouse Service Developer Guide*.

-  (Recommended) Use OBS foreign tables. Data files to be exported are specified based on the export mode and data format in OBS foreign table settings.
-  Use GDS. In high-concurrency scenarios, you can use GDS to export a large amount of data from a database to a common file system.
-  Use **gs_dump** and **gs_dumpall**. You can export a single database and its objects, or export all databases and common global objects in a cluster.

Fine-grained Policies
---------------------

In actual services, you may need to grant different operation permissions on resources to users of different roles. The IAM service provides fine-grained access control. An IAM administrator (a user in the **admin** group) can create a custom policy containing required permissions. After a policy is granted to a user group, users in the group can obtain all permissions defined by the policy. In this way, IAM implements fine-grained permission management. To control the DWS operations on resources more precisely, you can use the user management function of IAM to grant different operation permissions to users of different roles for fine-grained permission control. For details, see "Syntax of Fine-Grained Permission Policies" in *Data Warehouse Service User Guide*.

Separation of Duties
--------------------

Supports SSL network connections, user permission management, and password management, ensuring data security at the network, management, application, and system layers. You can assign the least permissions required for different roles to protect database objects from arbitrary addition, deletion, and modification. DWS supports the separation of rights, so that a group of database objects cannot be modified by multiple users, thus ensuring database security and data validity. You can further classify permissions by managing roles, schemas, and private users.

Row-Level Access Control
------------------------

The row-level access control feature is fine-grained. In this way, the same SQL query may return different results for different users. That is, you can allow users to view only certain rows in a table. For details, see "DWS Row-Level Access Control" in *Data Warehouse Service Developer Guide*.

Data Masking
------------

In the big data era, the increasing value of data has made privacy protection more challenging. Data masking makes it easier to mass data while protecting critical information. Customers can identify sensitive data based on their service scenarios and create data masking policies by column. After a data masking policy is created, only the administrator and table object owner can access the data. Data masking is applicable to industries involving sensitive information, such as finance, government, and healthcare. Data masking can be used to prevent sensitive information leakage in application development, testing, and training scenarios. For details, see "DWS Data Masking" in *Data Warehouse Service Developer Guide*.

Modifying DWS Cluster Parameters
--------------------------------

After a cluster is created, you can modify the cluster's database parameters as required. On the DWS management console, you can view or set common database parameters. For details, see "Modifying GUC Parameters of the DWS Cluster" in *Data Warehouse Service User Guide*.

DWS Snapshot Management
-----------------------

A snapshot is a complete backup that records point-in-time configuration data and service data of a DWS cluster. A snapshot can be used to restore a cluster at a certain time. You can manually create snapshots for a cluster or enable automated snapshot creation (periodic). Automated snapshots have a limited retention period. You can copy automatic snapshots for long-term retention.

When you restore a cluster from a snapshot, the system can restore the snapshot data to a new cluster or the original cluster.

You can delete snapshots that are no longer needed on the console to release storage space. Automated snapshots cannot be manually deleted.

For details, see "Backing Up and Restoring a DWS Cluster" in *Data Warehouse Service User Guide*.

DWS Resource Load Management
----------------------------

When multiple database users query jobs at the same time, some complex queries may occupy cluster resources for a long time, affecting the performance of other queries. For example, a group of database users continuously submit complex and time-consuming queries, while another group of users frequently submit short queries. In this case, short queries may have to wait in the queue for the time-consuming queries to complete. To improve efficiency, you can use the DWS resource management function to handle such problems. You can create different resource pools for different types of services, and configure different resource ratios for these pools. Then, add database users to the corresponding pools to restrict their resource usages. For details, see "DWS Resource Load Management" in *Data Warehouse Service User Guide*.

Managing DWS Logical Clusters
-----------------------------

A physical cluster can be divided into logical clusters that use the node-group mechanism. Tables in a database can be allocated to different physical nodes by logical cluster. A logical cluster can contain tables from multiple databases. For details, see "Managing DWS Logical Clusters" in *Data Warehouse Service User Guide*.

Upgrading a DWS Cluster
-----------------------

By default, you do not need to manually upgrade a DWS cluster. Clusters of version 8.1.1 and later allow you to deliver cluster upgrade operations on the console. For details, see "Upgrading a DWS Cluster" in *Data Warehouse Service User Guide*.

Starting, Stopping, and Deleting a DWS Cluster
----------------------------------------------

-  **Restarting a cluster**

   If a cluster is in the **Unbalanced** state or cannot work properly, you may need to restart it for restoration. After modifying a cluster's configurations, such as security settings and parameters, manually restart the cluster to make the configurations take effect.

-  **Stopping a cluster**

   If a cluster is no longer used, you can stop the cluster to bring services offline.

-  **Starting a cluster**

   You can start a stopped cluster to restore cluster services.

-  **Deleting a cluster**

   You can delete a cluster that you no longer need. For details, see "Starting, Stopping, and Deleting a DWS Cluster" in *Data Warehouse Service User Guide*.

DWS Cluster Monitoring
----------------------

-  Viewing DWS cluster monitoring information on Cloud Eye

   DWS integrates with Cloud Eye, allowing you to monitor compute nodes and databases in the cluster in real time. For details, see "Viewing DWS Cluster Monitoring Information on Cloud Eye" in *Data Warehouse Service User Guide*.

-  Viewing DWS cluster monitoring information on the monitoring panel (DMS)

   DMS is provided by DWS to ensure the fast and stable running of databases. It collects, monitors, and analyzes the disk, network, and OS metric data used by the service database, as well as key performance metric data of cluster running. It also diagnoses database hosts, instances, and service SQL statements based on the collected metrics to expose key faults and performance problems in a database in a timely manner, and guides customers to optimize and resolve the problems. For details, see "Viewing DWS Cluster Monitoring Information on the Monitoring Panel (DMS)" in *Data Warehouse Service User Guide*.

Event Management
----------------

DWS interconnects with Simple Message Notification (SMN) so that you can subscribe to events and view events that are triggered. For details, see "Viewing and Subscribing to DWS Cluster Events" in *Data Warehouse Service User Guide*.

Alarm Management
----------------

You can check and configure alarm rules and subscribe to alarm notifications. Alarm rules display alarm statistics and details of the past week for users to view tenant alarms. This feature monitors common DWS alarms with pre-set rules and allows users to customize the alarm thresholds based on their service needs. For details, see "Viewing and Subscribing to DWS Cluster Alarms" in *Data Warehouse Service User Guide*.

DWS Node Management
-------------------

You can check the nodes in a cluster, including the status, specifications, and usage of each node. To prepare for a large scale-out, you can add nodes in batches. To add 180 nodes, add them in three batches of 60 nodes each. If any nodes fail to be added, retry adding them. Once all 180 nodes are added, use them for scaling out. Adding nodes will not interrupt cluster services. For details, see "Managing Nodes" in *Data Warehouse Service User Guide*.

Scaling Out a DWS Cluster
-------------------------

As the service volume increases, the current scale of a cluster may not meet service requirements. In this case, you can scale out the cluster by adding compute nodes to it. Services are not interrupted during the scale-out. You can enable automatic redistribution if necessary. For details, see "Scaling Out a Cluster" in *Data Warehouse Service User Guide*.

DWS Redistribution Management
-----------------------------

By default, redistribution is automatically started after cluster scale-out. For enhanced reliability, disable the automatic redistribution function and manually start a redistribution task after the scale-out is successful. Data redistribution can accelerate service response. Currently, DWS supports offline redistribution (default mode). For details, see "Cluster Redistribution" in *Data Warehouse Service User Guide*.

Scaling In a DWS Cluster
------------------------

You can scale in your clusters on the console to release unnecessary compute and storage resources provided by DWS. For details, see "Scaling In a Cluster" in *Data Warehouse Service User Guide*.

Changing DWS Specifications
---------------------------

DWS supports elastic specifications changes. It is ideal for scenarios where computing capabilities (CPU and memory) need to be adjusted during peak hours or when only computing capabilities need to be changed. By using elastic flavor change before peak hours, the cluster's computing capability can be quickly increased. After peak hours, the cluster configuration can be reduced to minimize costs. For details, see "Using the Elastic Specification Change" in *Data Warehouse Service User Guide*.

DWS Disk Capacity Expansion
---------------------------

As customer services evolve, disk space often becomes the initial bottleneck. In scenarios where other resources are ample, the conventional scale-out process is not only time-consuming but also resource-inefficient. Disk capacity expansion can quickly increase storage without service interruption. You can expand the disk capacity when no other services are running. If the disk space is insufficient after the expansion, you can continue to expand the disk capacity. If the expansion fails, you can expand the disk capacity again. For details, see "Disk Scaling" in *Data Warehouse Service User Guide*.

DWS DR Management
-----------------

Automatic snapshots work with OBS to implement intra-region disaster recovery (DR). If the production cluster fails to provide read and write services due to natural disasters in the specified region or cluster internal faults, the DR cluster becomes the production cluster to ensure service continuity. For details, see "DWS Cluster DR Management" in *Data Warehouse Service User Guide*.

Handling Abnormal DWS Clusters
------------------------------

-  **Removing the read-only status**

   A cluster in read-only status does not allow write operations. You can remove this status on the management console. For details, see "Removing the Read-only Status" in *Data Warehouse Service User Guide*.

-  **Performing a primary/standby switchback**

   In the **Unbalanced** state, the number of primary instances on some nodes increases. As a result, the load pressure is high. In this case, you can perform a primary/standby switchback for the cluster during off-peak hours to improve performance. For details, see "Performing a Primary/Standby Switchback" in *Data Warehouse Service User Guide*.

Binding and Unbinding Load Balancers
------------------------------------

If the private IP address or EIP of a CN is used to connect to a cluster, the failure of this CN will lead to cluster connection failure. If a private domain name is used for connection, connection failures can be avoided by polling. However, private domain names cannot be used for public network access, and requests cannot be forwarded in the case of a CN failure. Therefore, ELB is used to avoid single CN failures. For details, see "Binding and Unbinding Load Balancers for a DWS Cluster" in *Data Warehouse Service User Guide*.

Adding or Deleting a CN in a DWS Cluster
----------------------------------------

After a cluster is created, the number of required CNs varies with service requirements. DWS allows you to add or delete CNs as needed. For details, see "Adding or Deleting a CN in a DWS Cluster" in *Data Warehouse Service User Guide*.

Reclaiming DWS Space Using Vacuum
---------------------------------

Intelligent O&M helps DWS users with O&M tasks. With this feature, you can specify the proper time window and number of tasks to execute based on the cluster workload. Besides, Intelligent O&M can adjust task execution policies according to service changes in a timely manner to reduce the impact on services. Periodic tasks and one-off tasks are supported, and you can configure the time window as required. For details, see "Reclaiming DWS Space Using Vacuum" in *Data Warehouse Service User Guide*.

Audit Logs
----------

DWS provides database audit logs, management console audit logs, and other logs for users to query service logs, analyze problems, and learn product security and performance. For details, see "DWS Cluster Log Management" in *Data Warehouse Service User Guide*.

-  DWS can be integrated with Cloud Trace Service (CTS) to audit management console operations and API calls.
-  DWS records all SQL operations, including connection attempts, query attempts, and database changes.
-  DWS interconnects with Log Tank Service (LTS). You can view collected cluster logs or dump logs on the LTS console. The following log types are supported: CN logs, DN logs, OS messages logs, audit logs, cms logs, gtm logs, Roach client logs, Roach server logs, upgrade logs, and scale-out logs.

SQL Syntax
----------

DWS provides comprehensive SQL capabilities. For details, see "SQL Syntax Reference" in *Data Warehouse Service Developer Guide*.

-  Supports ANSI/ISO SQL 92, SQL99, and SQL 2003 standards, stored procedures, GBK and UTF-8 character sets, and SQL standard functions and OLAP analysis functions.
-  Compatible with the PostgreSQL/Oracle/Teradata/MySQL ecosystem and supports interconnection with mainstream database ETL and BI tools provided by third-party vendors.
-  Supports roaring bitmaps and common functions used with them, which are widely used for user feature extraction, user profiling, and more applications in the Internet, retail, education, and gaming industries.
-  List partitioning (**PARTITION BY LIST** *(partition_key,[...])*) and range partitioning are supported.
-  Read-only HDFS and OBS foreign tables in JSON file format are supported.
-  DWS supports standard SQL syntax and DDL, DML, and DCL syntax.

   -  Data definition language (DDL) is used to define or modify an object in a database, such as a table, index, or view.
   -  Data Manipulation Language (DML) is used to perform operations on data in database tables, such as inserting, updating, querying, or deleting data.
   -  Data control language (DCL) is used to set or modify database users or role rights.

-  Permissions on system catalogs can be granted to common users. The **VACUUM** permission can be granted separately. Roles with predefined, extensible permissions are supported, including:

   -  **ALTER**, **DROP**, and **VACUUM** permissions at the table level.
   -  **ALTER** and **DROP** permissions at the schema level.
   -  Preset roles **role_signal_backend** and **role_read_all_stats**

Java UDF
--------

DWS supports Java user-defined functions (UDFs) to extend data processing capabilities. Developers can write UDFs in Java and use them in SQL queries or other data processing tasks. With the DWS PL/Java functions, you can choose your favorite Java IDE to write Java methods and install the JAR files containing these methods into the DWS database before invoking them. DWS PL/Java is developed based on open-source Greenplum PL/Java 1.4.0, which uses JDK 1.8.0_201. For details, see "DWS PL/Java Functions" in *Data Warehouse Service Developer Guide*.

Top SQL Query
-------------

DWS supports top SQL query, including real-time top SQL query and historical top SQL query. The real-time resource monitoring view records the resource usage (including memory, disk, CPU time, and I/O) and performance alarm information during job running. Based on the information, you can evaluate whether a query hits performance bottlenecks or affects the overall performance of the cluster.

-  Real-time top SQL query: For active SQL queries, you can check their real-time resource monitoring views at the query or operator level, and find the real-time top SQL queries whose execution cost is higher than **resource_track_cost**. For details, see "Real-time Top SQL" in *Data Warehouse Service Developer Guide*.
-  Historical top SQL query: You can trace information about historical jobs, including their resource usage (including memory, spill to disk, CPU time, and I/O), running status (including errors, termination, and exceptions), and performance alarms. You can check historical resource monitoring views at query and operator levels, and can find historical top SQL statements whose execution cost is higher than **resource_track_cost**. For details, see "Historical Top SQL" in *Data Warehouse Service Developer Guide*.

SQL Tuning
----------

The aim of SQL tuning is to maximize the utilization of resources, including CPU, memory, disk I/O, and network I/O. To maximize resources, you need to run SQL statements efficiently for high performance at a lower cost. For example, when performing a typical point query, you can use the seqscan and filter (that is, read every tuple and query conditions for match). You can also use an index scan, which can be implemented at a lower cost but achieve the same effect. For details, see "SQL Tuning" in *Data Warehouse Service Developer Guide*.

PostGIS Extension
-----------------

DWS provides PostGIS Extension (PostGIS-2.4.2). PostGIS Extension is a spatial database extender for PostgreSQL. It provides the following spatial information services: spatial objects, spatial indexes, spatial functions, and spatial operators. PostGIS Extension complies with the OpenGIS specifications. For details, see "Using PostGIS Extension" in *Data Warehouse Service Developer Guide*.

In DWS, PostGIS Extension depends on the listed third-party open-source software.

-  Geos 3.6.2
-  Proj 4.9.2
-  Json 0.12.1
-  Libxml2 2.7.1
-  Gdal 1.11.0

Roaring Bitmaps
---------------

DWS supports roaring bitmaps and common functions used with them, which are widely used for user feature extraction, user profiling, and more applications in the Internet, retail, education, and gaming industries. For details, see "Bitmap Functions and Operators" in *Data Warehouse Service Developer Guide*.

-  Before launching a sales promotion, e-commerce companies need to send the promotion message to a selected batch of users with specific features.
-  In education, exercise questions need to be pushed based on students' weaknesses.
-  On search, video, and portal websites, different contents are pushed to users based on their interests.

List Partitioning
-----------------

List partitioning (**PARTITION BY LIST** *(partition_key,[...])*) and range partitioning are supported.

In list partitioning, partition keys support the following data types: TINYINT, SMALLINT, INTEGER, BIGINT, NUMERIC/DECIMAL, TEXT, NVARCHAR2, VARCHAR(n), CHAR, BPCHAR, TIME, TIME WITH TIMEZONE, TIMESTAMP, TIMESTAMP WITH TIME ZONE, DATE, INTERVAL and SMALLDATETIME. For details, see "CREATE TABLE PARTITION" in *Data Warehouse Service Developer Guide*.

Materialized Views
------------------

A materialized view is a special database object. Materialized views calculate complex query results in advance and store the results to storage media. During service query, the pre-calculated data is directly queried to accelerate the query. Materialized views have advantages over common views in performance and storage persistence.

-  **Higher query performance**

   Materialized views pre-calculate and store query results. When a query is initiated, materialized views can be queried and directly return results, improving query performance.

-  **Lower compute resource consumption**

   Executing complex queries multiple times requires significant CPU and memory resources. Materialized views help reduce the load of database resources by avoiding these repeated calculations.

-  **Data warehouse modeling**

   Materialized views can be used as the data access layer to support complex service logic and allow users to access preprocessed data without considering the complex SQL logic at the bottom layer.

For details, see "DWS Materialized Views" in *Data Warehouse Service Developer Guide*.

SQL on Hudi
-----------

DWS clusters of 9.1.0.100 and later versions access Hudi data on OBS. Hadoop Upserts Deletes and Incrementals (Hudi) is an open-source data lake table format and data processing framework. It is designed for large data lakes and supports efficient upserts, deletion, and incremental processing. It is not merely a data format or data access method, but a combination of the two that provides a comprehensive solution for managing data in data lakes. DWS provides a series of system functions to obtain Hudi foreign table information and create Hudi automatic synchronization tasks. For details, see "SQL on Hudi" in *Data Warehouse Service Developer Guide*.
