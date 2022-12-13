:original_name: dws_01_0002.html

.. _dws_01_0002:

What Is GaussDB(DWS)?
=====================

GaussDB(DWS) is an online data processing database that runs on the cloud infrastructure to provide scalable, fully-managed, and out-of-the-box analytic database service, freeing you from complex database management and monitoring. It is a native cloud service based on the converged data warehouse GaussDB, and is fully compatible with the standard ANSI SQL 99 and SQL 2003, as well as the PostgreSQL and Oracle ecosystems. GaussDB(DWS) provides competitive solutions for PB-level big data analysis in various industries.

Architecture
------------

GaussDB(DWS) employs the shared-nothing architecture and the massively parallel processing (MPP) engine, and consists of numerous independent logical nodes that do not share the system resources such as CPUs, memory, and storage. In such a system architecture, service data is separately stored on numerous nodes. Data analysis tasks are executed in parallel on the nodes where data is stored. The massively parallel data processing significantly improves response speed.


.. figure:: /_static/images/en-us_image_0000001180320405.png
   :alt: **Figure 1** Architecture

   **Figure 1** Architecture

-  **Application layer**

   Data loading tools, extract, transform, and load (ETL) tools, business intelligence (BI) tools, as well as data mining and analysis tools, can be integrated with GaussDB(DWS) through standard APIs. GaussDB(DWS) is compatible with the PostgreSQL ecosystem, and the SQL syntax is compatible with Oracle and Teradata. Applications can be smoothly migrated to GaussDB(DWS) with few changes.

-  **API**

   Applications can connect to GaussDB(DWS) through the standard Java Database Connectivity (JDBC) 4.0 and Open Database Connectivity (ODBC) 3.5.

-  **GaussDB(DWS) (MPP cluster)**

   A GaussDB(DWS) cluster contains nodes of the same flavor in the same subnet. These nodes jointly provide services. Datanodes (DNs) in a cluster store data on disks. Coordinators (CNs) receive access requests from applications and return the execution results to clients. In addition, a CN splits and distributes tasks to the DNs for parallel processing.

-  **Automatic data backup**

   Cluster snapshots can be automatically backed up to the EB-level Object Storage Service (OBS), which facilitates periodic backup of the cluster during off-peak hours, ensuring data recovery after a cluster exception occurs.

   A snapshot is a complete backup of GaussDB(DWS) at a specific time point, including the configuration data and service data of a cluster.

-  **Tool chain**

   The parallel data loading tool General Data Service (GDS), SQL syntax migration tool Database Schema Convertor (DSC), and SQL development tool Data Studio are provided. The cluster O&M can be monitored on a console.

Logical Cluster Architecture
----------------------------

:ref:`Figure 2 <en-us_topic_0000001134400792__fig9635133716155>` shows the logical architecture of a GaussDB(DWS) cluster. For details about instances, see :ref:`Table 1 <en-us_topic_0000001134400792__en-us_topic_0059778972_tf208b79c11514cb9ae9d57ba2bef8ec9>`.

.. _en-us_topic_0000001134400792__fig9635133716155:

.. figure:: /_static/images/en-us_image_0000001154106106.jpg
   :alt: **Figure 2** Logical cluster architecture

   **Figure 2** Logical cluster architecture

.. _en-us_topic_0000001134400792__en-us_topic_0059778972_tf208b79c11514cb9ae9d57ba2bef8ec9:

.. table:: **Table 1** Cluster architecture description

   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name                             | Description                                                                                                                                                                       | Remarks                                                                                                                                                                                                                                                                                          |
   +==================================+===================================================================================================================================================================================+==================================================================================================================================================================================================================================================================================================+
   | Global Transaction Manager (GTM) | Generates and maintains the globally unique information, such as the transaction ID, transaction snapshot, and timestamp.                                                         | The cluster includes only one pair of GTMs: one primary GTM and one standby GTM.                                                                                                                                                                                                                 |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Workload Manager (WLM)           | WLM controls allocation of system resources to prevent service congestion and system crash resulting from excessive workload.                                                     | You do not need to specify names of hosts where WLMs are to be deployed, because the installation program automatically installs a WLM on each host.                                                                                                                                             |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Coordinator (CN)                 | A CN receives access requests from applications, and returns execution results to the client; splits tasks and allocates task fragments to different DNs for parallel processing. | CNs in a cluster have equivalent roles and return the same result for the same DML statement. Load balancers can be added between CNs and applications to ensure that CNs are transparent to applications. If a CN is faulty, the load balancer connects its applications to another CN.         |
   |                                  |                                                                                                                                                                                   |                                                                                                                                                                                                                                                                                                  |
   |                                  |                                                                                                                                                                                   | CNs need to connect to each other in the distributed transaction architecture. To reduce heavy load caused by excessive threads on GTMs, no more than 10 CNs should be configured in a cluster.                                                                                                  |
   |                                  |                                                                                                                                                                                   |                                                                                                                                                                                                                                                                                                  |
   |                                  |                                                                                                                                                                                   | GaussDB(DWS) handles the global resource load in a cluster using the Central Coordinator (CCN) for adaptive dynamic load management. When the cluster is started for the first time, the CM selects the CN with the smallest ID as the CCN. If the CCN is faulty, CM replaces it with a new one. |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Datanode (DN)                    | A DN stores service data by column or row or in the hybrid mode, executes data query tasks, and returns execution results to CNs.                                                 | A cluster consists of multiple DNs and each DN stores part of data. If no HA solution is available for DNs, data cannot be accessed when a DN is faulty.                                                                                                                                         |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Storage                          | Functions as the server's local storage resources to store data permanently.                                                                                                      | ``-``                                                                                                                                                                                                                                                                                            |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

DNs in a cluster store data on disks. :ref:`Figure 3 <en-us_topic_0000001134400792__fig138416215395>` describes the objects on each DN and the relationships among them logically.

-  A database manages various data objects and is isolated from other databases.
-  A datafile segment stores data in only one table. A table containing more than 1 GB of data is stored in multiple data file segments.
-  A table belongs only to one database.
-  A block is the basic unit of database management, with a default size of 8 KB.

Data can be distributed in replication, round-robin, or hash mode. You can specify the distribution mode during table creation.

.. _en-us_topic_0000001134400792__fig138416215395:

.. figure:: /_static/images/en-us_image_0000001200065787.png
   :alt: **Figure 3** Logical database architecture

   **Figure 3** Logical database architecture
