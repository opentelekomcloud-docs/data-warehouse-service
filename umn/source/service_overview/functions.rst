:original_name: dws_01_0091.html

.. _dws_01_0091:

Functions
=========

GaussDB(DWS) enables you to use this service through various methods, such as the GaussDB(DWS) management console, GaussDB(DWS) client, and REST APIs. This section describes the main functions of GaussDB(DWS).

Enterprise-Level Data Warehouses and Compatibility with Standard SQL
--------------------------------------------------------------------

After a data warehouse cluster is created, you can use the SQL client to connect to the cluster and perform operations such as creating a database, managing the database, importing and exporting data, and querying data.

GaussDB(DWS) provides petabyte-level (PB-level) high-performance databases with the following features:

-  MPP computing framework, hybrid row-column storage, and vectorized execution, enabling response to billion-level data correlation analysis within seconds
-  Optimized in-memory computing based on Hash Join of Bloom Filter, improving the performance by 2 to 10 times
-  Supports the symmetrically distributed, active-active multi-node cluster architecture, ensuring no SPOFs.

-  Optimized communication between large-scale clusters based on telecommunication technologies, improving data transmission efficiency between compute nodes
-  Cost-based intelligent optimizers, helping generate the optimal plan based on the cluster scale and data volume to improve execution efficiency

GaussDB(DWS) has comprehensive SQL capabilities:

-  Supports ANSI/ISO SQL 92, SQL99, and SQL 2003 standards, stored procedures, GBK and UTF-8 character sets, and SQL standard functions and OLAP analysis functions.
-  Compatible with the PostgreSQL/Oracle/Teradata/MySQL ecosystem and supports interconnection with mainstream database ETL and BI tools provided by third-party vendors.
-  Supports roaring bitmaps and common functions used with them, which are widely used for user feature extraction, user profiling, and more applications in the Internet, retail, education, and gaming industries.
-  List partitioning (**PARTITION BY LIST** *(partition_key,[...])*) and range partitioning are supported.
-  Read-only HDFS and OBS foreign tables in JSON file format are supported.
-  Permissions on system catalogs can be granted to common users. The **VACUUM** permission can be granted separately. Roles with predefined, extensible permissions are supported, including:

   -  **ALTER**, **DROP** and **VACUUM** permissions at table level
   -  **ALTER** and **DROP** permissions at schema level
   -  Preset roles **role_signal_backend** and **role_read_all_stats**

For details about the SQL syntax and database operation guidance, see the *Data Warehouse Service Database Development Guide*.

Cluster Management
------------------

A data warehouse cluster contains nodes with the same flavor in the same subnet. These nodes jointly provide services. GaussDB(DWS) provides a professional, efficient, and centralized management console, allowing you to quickly apply for clusters, easily manage data warehouses, and focus on data and services.

Main functions of cluster management are described as follows:

-  Creating Clusters

   To use data warehouse services on the cloud, create a GaussDB(DWS) cluster first. You can select product and node specifications to quickly create a cluster.

-  Managing Snapshots

   A snapshot is a complete backup that records point-in-time configuration data and service data of a GaussDB(DWS) cluster. A snapshot can be used to restore a cluster at a certain time. You can manually create snapshots for a cluster or enable automated snapshot creation (periodic). Automated snapshots have a limited retention period. You can copy automatic snapshots for long-term retention.

   When you restore a cluster from a snapshot, the system can restore the snapshot data to a new cluster or the original cluster.

   You can delete snapshots that are no longer needed on the console to release storage space. Automated snapshots cannot be manually deleted.

-  Managing nodes

   You can check the nodes in a cluster, including the status, specifications, and usage of each node. To prepare for a large scale-out, you can add nodes in batches. To add 180 nodes, add them in three batches of 60 nodes each. If any nodes fail to be added, retry adding them. Once all 180 nodes are added, use them for scaling out. Adding nodes will not disrupt cluster services.

-  Scaling out clusters

   As the service volume increases, the current scale of a cluster may not meet service requirements. In this case, you can scale out the cluster by adding compute nodes to it. Services are not interrupted during the scale-out. You can enable online scale-out and automatic redistribution if necessary.

-  Managing redistribution

   By default, redistribution is automatically started after cluster scale-out. For enhanced reliability, disable the automatic redistribution function and manually start a redistribution task after the scale-out is successful. Data redistribution can accelerate service response. Currently, offline redistribution, online redistribution, and offline scheduling are supported. The default mode is offline redistribution.

-  Resource management

   When multiple database users query jobs at the same time, some complex queries may occupy cluster resources for a long time, affecting the performance of other queries. For example, a group of database users continuously submit complex and time-consuming queries, while another group of users frequently submit short queries. In this case, short queries may have to wait in the queue for the time-consuming queries to complete. To improve efficiency, you can use the GaussDB(DWS) resource management function to handle such problems. You can create different resource pools for different types of services, and configure different resource ratios for these pools. Then, add database users to the corresponding pools to restrict their resource usages.

-  Logical cluster

   A physical cluster can be divided into logical clusters that use the node-group mechanism. Tables in a database can be allocated to different physical nodes by logical cluster. A logical cluster can contain tables from multiple databases.

-  Restarting clusters

   Restarting a cluster may cause data loss in running services. If you have to restart a cluster, ensure that there is no running service and all data has been saved.

-  Deleting Clusters

   You can delete a cluster when you do not need it. Deleting a cluster is risky and may cause data loss. Therefore, exercise caution when performing this operation.

GaussDB(DWS) allows you to manage clusters in either of the following ways:

-  Management console

   Use the management console to access GaussDB(DWS) clusters. When you have registered an account, log in to the management console and choose **Data Warehouse Service**.

   For more information about cluster management, see "Cluster Management" in the *Data Warehouse Service User Guide*.

-  REST APIs

   Use REST APIs provided by GaussDB(DWS) to manage clusters. In addition, if you need to integrate GaussDB(DWS) into a third-party system for secondary development, use APIs to access the service.

   For details, see the *Data Warehouse Service API Reference*.

Diverse Data Import Modes
-------------------------

GaussDB(DWS) supports efficient data import from multiple data sources. The following lists typical data import modes. For details, see "Data Migration to GaussDB(DWS)" in *Data Warehouse Service (DWS) Developer Guide*.

-  Importing data from OBS in parallel
-  Using GDS to import data from a remote server
-  Importing data from MRS to a data warehouse cluster
-  Importing data from one GaussDB(DWS) cluster to another
-  Using the gsql meta-command **\\COPY** to import data
-  Running the **COPY FROM STDIN** statement to import data
-  Using Database Schema Convertor (DSC) to migrate SQL scripts
-  Using **gs_dump** and **gs_dumpall** to export metadata
-  Using **gs_restore** to import data

APIs
----

You can call standard APIs, such as JDBC and ODBC, to access databases in GaussDB(DWS) clusters.

For details, see "Using the JDBC and ODBC Drivers to Connect to a Cluster" in the *Data Warehouse Service (DWS) User Guide*.

High Reliability
----------------

-  Supports instance and data redundancy, ensuring zero single points of failure (SPOF) in the entire system.
-  Supports multiple data backups, and all data can be manually backed up to OBS.
-  Automatically isolates the faulty node, uses the backup to restore data, and replaces the faulty node when necessary.
-  Automatic snapshots work with OBS to implement intra-region disaster recovery (DR). If the production cluster fails to provide read and write services due to natural disasters in the specified region or cluster internal faults, the DR cluster becomes the production cluster to ensure service continuity.
-  In the **Unbalanced** state, the number of primary instances on some nodes increases. As a result, the load pressure is high. In this case, you can perform a primary/standby switchback for the cluster during off-peak hours to improve performance.
-  If the internal IP address or EIP of a CN is used to connect to a cluster, the failure of this CN will lead to cluster connection failure. To avoid single-CN failures, GaussDB(DWS) uses Elastic Load Balance (ELB). An ELB distributes access traffic to multiple ECSs for traffic control based on forwarding policies. It improves the fault tolerance capability of application programs.
-  After a cluster is created, the number of required CNs varies with service requirements. GaussDB(DWS) allows you to add or delete CNs as needed.

Security Management
-------------------

-  Isolates tenants and controls access permissions to protect the privacy and data security of systems and users based on the network isolation and security group rules, as well as security hardening measures.
-  Supports SSL network connections, user permission management, and password management, ensuring data security at the network, management, application, and system layers.

Monitoring and Auditing
-----------------------

-  Monitoring Clusters

   GaussDB(DWS) integrates with Cloud Eye, allowing you to monitor compute nodes and databases in the cluster in real time. For details, see "Cluster Monitoring" in *Data Warehouse Service (DWS) User Guide*.

-  Database Monitoring

   DMS is provided by GaussDB(DWS) to ensure the fast and stable running of databases. It collects, monitors, and analyzes the disk, network, and OS metric data used by the service database, as well as key performance metric data of cluster running. It also diagnoses database hosts, instances, and service SQL statements based on the collected metrics to expose key faults and performance problems in a database in a timely manner, and guides customers to optimize and resolve the problems. For details, see "Database Monitoring" in *Data Warehouse Service (DWS) User Guide*.

-  Alarms

   Alarm management includes viewing and configuring alarm rules and subscribing to alarm information. Alarm rules display alarm statistics and details of the past week for users to view tenant alarms. In addition to providing a set of default GaussDB(DWS) alarm rules, this feature allows you to modify alarm thresholds based on your own services. For details, see "Alarms" in *Data Warehouse Service (DWS) User Guide*.

-  Audit Logs

   -  GaussDB(DWS) integrates with Cloud Trace Service (CTS), allowing you to audit operations performed on the management console and API invocation operations. For details, see "Viewing Audit Logs of Key Operations on the Management Console".
   -  GaussDB(DWS) records all SQL operations, including connection attempts, query attempts, and database changes. For details, see "Configuring the Database Audit Logs" in *Data Warehouse Service (DWS) User Guide*.

Multiple Database Tools
-----------------------

GaussDB(DWS) provides the following self-developed tools. You can download the tool packages on the GaussDB(DWS) management console. For how to use the tools, see the *Data Warehouse Service (DWS) Tool Guide*.

-  gsql

   gsql is a command line SQL client tool running on the Linux operating system. It helps connect to, operate, and maintain the database in a data warehouse cluster.

-  Data Studio

   Data Studio is a Graphical User Interface (GUI) SQL client tool running on the Windows operating system. It is used to connect to the database in a data warehouse cluster, manage the database and database objects, edit, run, and debug SQL scripts, and view the execution plans.

-  GDS

   GDS is a data service tool provided by GaussDB(DWS). It works with the foreign table mechanism to implement high-speed data import and export.

   The GDS tool package needs to be installed on the server where the data source file is located. This server is called the data server or the GDS server.

-  DSC SQL syntax migration tool

   The DSC is a command-line tool running on the Linux or Windows OS. It is dedicated to providing customers with simple, fast, reliable application SQL script migration services. It parses SQL scripts of source database applications by using the built-in syntax migration logic, and migrates them to be applicable to GaussDB(DWS) databases.

   The DSC can migrate SQL scripts of Teradata, Oracle, Netezza, MySQL, and DB2 databases.

-  **gs_dump** and **gs_dumpall**

   **gs_dump** exports a single database or its objects. **gs_dumpall** exports all databases or global objects in a cluster.

   To migrate database information, you can use a tool to import the exported metadata to a target database.

-  gs_restore

   During database migration, you can export files using **gs_dump tool** and import them to GaussDB(DWS) by using **gs_restore**. In this way, metadata, such as table definitions and database object definitions, can be imported.
