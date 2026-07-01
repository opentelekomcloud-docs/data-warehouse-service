:original_name: dws_01_0004.html

.. _dws_01_0004:

Concepts
========

DWS Management Concepts
-----------------------

-  **Cluster**

   A cluster is a server group that consists of multiple nodes. DWS is organized using clusters. A data warehouse cluster contains nodes with the same flavor in the same subnet. These nodes work together to provide services.

-  **Node**

   A DWS cluster can contain 3 to 256 nodes. Each node can store and analyze data. For details, see :ref:`DWS Technical Specifications <dws_01_00113>`.

-  Specifications

   You need to specify the node flavors when you create a DWS cluster. CPU, memory, and storage resources vary depending on node flavors.

-  **Snapshot**

   You can create snapshots to back up DWS cluster data. A snapshot is retained until you delete it on the management console. Automated snapshots cannot be manually deleted. Snapshots will occupy your OBS quotas.

-  **Project**

   Projects are used to group and isolate OpenStack resources (computing resources, storage resources, and network resources). A project can be a department or a project team. Multiple projects can be created for one account.

DWS Database Concepts
---------------------

-  **Database**

   A database manages data objects and is isolated from other databases. While creating an object, you can specify a tablespace for it. If you do not specify it, the object will be saved to the **PG_DEFAULT** space by default. Objects managed by a database can be distributed to multiple tablespaces.

-  **OLAP**

   OLAP is a major function of DWS clusters. It supports complex analysis, provides decision-making support tailored to analysis results, and delivers intuitive query results.

-  **MPP**

   On each node in the data warehouse cluster, memory computing and disk storage systems are independent from each other. With MPP, DWS distributes service data to different nodes based on the database model and application characteristics. Nodes are connected through the network and collaboratively process computing tasks as a cluster and provide database services that meet service needs.

-  **Shared-nothing architecture**

   The shared-nothing architecture is a distributed computing architecture. Each node is independent so that nodes do not compete for resources, which improves work efficiency.

-  **Database version**

   Each data warehouse cluster has a specific database version. You can check the version when creating a data warehouse cluster.

-  **Database connections**

   You can use a client to connect to the DWS cluster. The client can be used for connection on the cloud platform and over the Internet.

-  **Database users and roles**

   DWS uses users and roles to control the access to databases. A role can be a database user or a group of database users based on the role setting. In DWS, the difference between roles and users is that a role does not have the **LOGIN** permission by default. In DWS, one user can have only one role, but you can put a user's role under a parent role to grant multiple permissions to the user.

-  **Instance**

   In DWS, instances are a group of database processes running in the memory. An instance can manage one or more databases that form a cluster. A cluster is an area in the storage disk. This area is initialized during installation and composed of a directory. The directory, called data directory, stores all data and is created by **initdb**. Theoretically, one server can start multiple instances on different ports, but DWS manages only one instance at a time. The start and stop of an instance rely on the specific data directory. For compatibility purposes, the concept of instance name may be introduced.

-  **Tablespace**

   DWS only supports relative tablespaces. A tablespace is a subdirectory under the data directory of a database instance. There may be multiple tablespaces that store the physical files of the database. Files are physically isolated using tablespaces and managed by a file system.

-  **Schema**

   DWS schemas logically separate databases. All database objects are created under certain schemas. In DWS, schemas and users are loosely bound. When you create a user, a schema with the same name as the user will be created automatically. You can also create a schema or specify another schema.

-  **V2 table**

   A V2 table refers to a table whose **colversion** defined in the **CREATE TABLE** syntax is 2.0 during table creation, indicating that each column of the column-store table is combined and stored in a file named **relfilenode.C1.0**, and data is stored on the local disk. For storage-compute coupled clusters, if **colversion** is not specified, the created column-store table is a V2 table by default.

-  **V3 table**

   A V3 table refers to a table whose **colversion** defined in the **CREATE TABLE** syntax is 3.0 during table creation, indicating that each column of the column-store table is stored in a file named **C1_field.0**, and data is stored in the OBS file system. For storage-compute coupled clusters, if **colversion** is not specified, the created column-store table is a V3 table by default.

-  **Transaction management**

   In DWS, transactions are managed by multi-version concurrency control (MVCC) and two-phase locking (2PL). It enables smooth data reads and writes. In DWS, MVCC saves historical version data together with the current tuple version. DWS uses the VACUUM process instead of rollback segments to routinely delete historical version data. This does not affect user operations, unless in performance tuning. Transactions are automatically submitted in DWS.

-  **Node Group**

   A node group is a basic concept of the database kernel and contains multiple cluster nodes. A physical cluster contains multiple node groups, and each node group is a logical cluster that can be accessed by users.

-  **Logical Cluster**

   In the storage-compute coupled architecture, each physical cluster is divided into multiple node groups, and each node group is a logical cluster.
