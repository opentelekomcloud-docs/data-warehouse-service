:original_name: dws_01_7241.html

.. _dws_01_7241:

Logical Cluster Overview
========================

Concepts
--------

A physical cluster can be divided into Node Groups, which are logical clusters. All physical nodes in a physical cluster are divided into multiple logical clusters. A logical cluster is essentially a node group that contains one or more physical nodes. Each physical node belongs to only one logical cluster, and user data tables can only be distributed within the same logical cluster. The data of each logical cluster is isolated from the others. The physical resources allocated to a logical cluster are mainly used for operations on its own data tables, but also for interactive queries with other logical clusters. An enterprise can deploy services on different logical clusters to implement unified service management, and meanwhile isolate the data and resources of services.

Logical clusters are created by dividing nodes of a physical cluster. Tables in a database can be allocated to different physical nodes by logical cluster. A logical cluster can contain tables from multiple databases. :ref:`Figure 1 <en-us_topic_0000002203312253__fig41828596595>` shows the relationships between logical clusters, databases, and tables.

An elastic cluster is a cluster that always exists in logical cluster mode and consists of nodes that are not part of any logical cluster. It is a special node group that can have multiple or zero DNs. An elastic cluster cannot be manually created. When the first logical cluster is created in a physical cluster, an elastic cluster is also automatically created and all physical nodes not belonging to the logical cluster are automatically added to the elastic cluster. DNs in the elastic cluster will be used for logical clusters created later. To create a logical cluster, ensure that your logical cluster has DNs. (DNs are not required only when you create the first logical cluster in physical cluster mode.) You can add new physical nodes to the elastic cluster through scale-out.

.. _en-us_topic_0000002203312253__fig41828596595:

.. figure:: /_static/images/en-us_image_0000002203427233.png
   :alt: **Figure 1** Relationships between logical clusters, databases, and tables

   **Figure 1** Relationships between logical clusters, databases, and tables

.. note::

   -  Logical clusters are supported in 8.1.0.100 or later.
   -  You are advised to allocate tables in a database to the same logical cluster.
   -  A logical cluster is not an independent sub-cluster. It can isolate data, resource, and permissions, but cannot be independently operated or maintained.
   -  The **Change all specifications** option does not support logical clusters.
   -  If the original physical cluster contains data, it is not possible to switch the logical cluster of a cluster. Ensure the original physical cluster is empty during the switchover.

Logical Cluster Architecture
----------------------------

:ref:`Figure 2 <en-us_topic_0000002203312253__fig141362033122319>` shows the architecture of a physical cluster divided into multiple logical clusters. Nodes in the physical cluster are divided into Node Groups. The jobs of users 1 and 2 are executed in different Node Groups. The two users can define resource pools within their own logical cluster to control resources (CPU, memory, and I/O) used for different jobs. If some jobs of user 1 need to access the data of user 2, they can access data across Node Groups after being authorized. For a logical cluster, you can configure resources accessible across logical clusters to ensure its resources are sufficient.

.. _en-us_topic_0000002203312253__fig141362033122319:

.. figure:: /_static/images/en-us_image_0000002167906548.png
   :alt: **Figure 2** Logical Cluster Architecture

   **Figure 2** Logical Cluster Architecture

A physical cluster is divided into multiple logical ones. You can define a resource pool for each of them based on service requirements. User tables are not distributed across logical clusters. If services do not access data across logical clusters, they will not compete for resources. Resources can be allocated to jobs in the same logical cluster by using resource pools. If necessary, you can let services access data across logical clusters, and control the resources used for such access to reduce resource competition between jobs within and outside a logical cluster.

After creating a physical cluster, you need to decide whether to divide it into logical clusters. You cannot divide it into logical clusters if you have already created user tables before, because these user tables are distributed on all physical nodes. For more information about the limitations, see :ref:`Constraints and Limitations <en-us_topic_0000002203312253__section1313312397191>`. If you want to manage an existing cluster (for example, a database cluster built in a version earlier than 8.1.0.100) as a logical cluster, you can upgrade the cluster to 8.1.0.100 or later and then convert all the nodes in the cluster into a single logical cluster. Then, add nodes to the physical cluster and create another logical cluster on the new nodes.

Operations on logical clusters include:

-  :ref:`Adding/Deleting a Logical Cluster <dws_01_7242>`:

   -  Creating a logical cluster: After converting a physical cluster into a logical cluster, you can group some physical nodes into a logical cluster by specifying the name and the nodes of the logical cluster.
   -  Deleting a logical cluster: You can delete a logical cluster with a specified name. After the logical cluster is deleted, the released physical nodes are removed from the physical cluster.

-  :ref:`Managing Logical Clusters <dws_01_7243>`:

   -  Modifying a logical cluster: You can add or remove nodes from a logical cluster as needed.
   -  Resource management (logical cluster mode): You can manage resources in a specified logical cluster (supported only by 8.1.3.\ *x* and later versions).
   -  Scaling out a logical cluster: This operation increases the number of physical nodes in the logical cluster and redistributes tables in the logical cluster to the new physical nodes.
   -  Restarting a logical cluster: This operation restarts all DNs in the logical cluster. Considering the impact on the entire physical cluster, the DNs in a logical cluster cannot be stopped or started individually.
   -  Scaling in a logical cluster: Select and scale in a host ring from an elastic cluster.

.. _en-us_topic_0000002203312253__section1313312397191:

Constraints and Limitations
---------------------------

-  The smallest unit of the creation, scale-out, and scale-in of a logical cluster is a ring. A ring consists of at least three hosts, where the primary, standby, and secondary DNs are deployed.
-  During the logical cluster switchover, if the original physical cluster has data, the cluster will be locked. You can run simple DML statements, such as adding, deleting, modifying, and querying data. However, running complex DDL statements, such as operating database objects, will block services and report errors. Exercise caution when performing this operation.
-  A logical cluster cannot be independently backed up or restored.
-  A logical cluster cannot be independently upgraded.
-  A physical cluster cannot be rolled back to a physical cluster after it is converted to a logical cluster.
-  In logical cluster mode, only logical clusters can be created, and Node Groups cannot be created. In addition, Node Groups cannot be created in a logical cluster.
-  O&M operations (creation, deletion, editing, scale-out, scale-in, and restart) of logical clusters cannot be performed concurrently.
-  Public database objects (excluding system catalogs, foreign tables, and views) are distributed on all nodes in a physical cluster. After a node of the logical cluster is restarted, the DDL operations performed by other logical clusters on the objects will be interrupted.
-  In logical cluster mode, each DN only contains the tables in the logical cluster that the DN belongs to. User-defined functions need to be created on all DNs. Therefore, **%type** cannot be used to reference table field types in the function body.
-  In logical cluster mode, the **WITH RECURSIVE** statement cannot be pushed down.
-  In logical cluster mode, partitions can be swapped only in the same logical cluster. Partitioned tables and common tables in different logical clusters cannot be swapped.
-  In logical cluster mode, if the function parameters or return values contain table types, these table types must belong to the same logical cluster.
-  In logical cluster mode, when you create a foreign table using **CREATE TABLE... LIKE**, the source table and the foreign table to be created must be in the same logical cluster.
-  In logical cluster mode, tables cannot be created schemas (by using **CREATE SCHEMA... CREATE TABLE** statements). Create a schema, and then create tables in the schema.
-  A logical cluster does not support the architecture of one primary node and multiple standby nodes. A logical cluster takes effect only in the architecture of one primary node, one standby node, and one secondary node.
-  A logical cluster user cannot access the global temporary tables created by another logical cluster user.
-  When a subquery in a query statement is in a different execution cluster than the parent query and includes filter criteria related to the parent query, the filter criteria cannot be applied across clusters in logical cluster mode. This leads to additional operator overhead, potentially resulting in poorer performance compared to a single-cluster setup.

Required Permissions on Tools
-----------------------------

The following describes user permissions for database objects in logical clusters:

-  The **CREATE ON NODE GROUP** permission can be granted to any user or role for performing operations such as creating tables in a logical cluster.

   -  If the schema specified for a created table is a private schema of a user (that is, the schema has the same name as the user and the owner of the schema is the user), the owner of the created table defaults to the user. You do not need to associate the table with a logical cluster.
   -  When a user associated with a logical cluster creates a table, if the **to group** clause is not specified, the table will be created in that logical cluster. The logical cluster associated with the user can be changed.
   -  If a user is not associated with any logical cluster, when the user creates a table, the table will be created in the logical cluster specified by **default_storage_nodegroup**. If **default_storage_nodegroup** is set to **installation**, the table will be created in the first logical cluster. In logical cluster mode, the logical cluster with the smallest OID is set as the first logical cluster. If **default_storage_nodegroup** is not set, its value is **installation** by default.
   -  The system administrator can run the **ALTER ROLE** command to set the default storage nodegroup for each user. For details about the syntax, see "ALTER ROLE" in the *Data Warehouse Service (DWS) SQL Syntax Reference*

-  .. _en-us_topic_0000002203312253__li591882483418:

   Table creation rules

   -  If **to group** is not specified for a user table but **default_storage_nodegroup** is set, tables will be created in the specified logical cluster.
   -  If **default_storage_nodegroup** is set to **installation**, tables will be created in the first logical cluster, that is, the logical cluster with the smallest OID.

-  The owner of a table can be changed to any user. However, you need to check the schema and node group permissions when performing operations on the table.

-  A system administrator can be associated with a logical cluster and can create tables in multiple logical clusters.

   -  If the system administrator is associated with a logical cluster and **to group** is not specified when you create a table, the table will be created in the associated logical cluster by default. If **to group** is specified, the table is created in the specified logical cluster.
   -  If the system administrator is not associated with a logical cluster and **to group** is not specified, tables are created in the logical cluster of **default_storage_nodegroup**. For details, see the :ref:`table creation rules <en-us_topic_0000002203312253__li591882483418>`.

-  System administrator permissions can be granted to a user associated with a logical cluster, but the :ref:`table creation rules <en-us_topic_0000002203312253__li591882483418>` also apply.
-  The logical cluster permission for accessing non-table objects (such as schemas/sequences/functions/triggers) will not be checked.
-  A resource pool must be associated with a logical cluster.

   -  A logical cluster can be associated with multiple resource pools but a resource pool can be associated with only one logical cluster.
   -  Jobs executed by logical cluster users associated with a resource pool can only use resources in the resource pool.
   -  You do not need to create a workload group to define the number of concurrent jobs in a logical cluster. Therefore, workload groups are not required for logical clusters.

-  When a logical cluster is deleted, only the table, foreign table, and resource pool objects are deleted.

   -  Objects dependent on the tables (including the partly dependent sequences/functions/triggers) in the logical cluster will also be deleted.
   -  Logical cluster associations with its users and parent-child tenants will be removed during the process. As a result, the users will be associated with the default **installation** node group and with the default global resource pool.

-  A logical cluster user can create a database if granted the permission.

Replication Table Node Group
----------------------------

A replication table node group is a special node group in logical cluster mode. It can contain one or more logical clusters, but can only create replication tables. One typical scenario is to create public dimension tables. If multiple logical clusters require some common dimension tables, create a replication table node group and add the common dimension tables to it. The logical clusters contained in the replication table node group can access these dimension tables on the local DNs, with no need to access the tables on other DNs. If a logical cluster is scaled in, the replication table node group will be scaled accordingly. If the logical cluster is deleted, the replication table node group will be scaled in. However, if the replication table node group contains only one logical cluster and the logical cluster is deleted, the replication table node group will also be deleted. In this case, create tables in a logical cluster instead.

Create a replication table node group using the **CREATE NODE GROUP** SQL statement and delete one using **DROP NODE GROUP**. Before deleting a replication table node group, delete all table objects in the node group.

.. note::

   Creation of replication table node groups is supported in 8.1.2 or later.

Application Scenarios
---------------------

**Scenario 1: Isolating data with different resource requirements**


.. figure:: /_static/images/en-us_image_0000002167906552.png
   :alt: **Figure 3** Logical cluster division based on resource requirements

   **Figure 3** Logical cluster division based on resource requirements

As shown in the preceding figure, data with different resource requirements is stored in different logical clusters, and different logical clusters also support mutual access. This ensures that functions are not affected while resources are isolated.

-  Tables T1 and T2 are used to calculate a large amount of data and generate report data (for example, bank batch processing). This process involves large batch import and big data query, which consume a lot of memory and I/O resources of nodes and take a long time. However, such a query does not require high real-time performance. Therefore, the data of these two tables can be separated into a different logical cluster.
-  Tables T3 and T4 contain some computing data and real-time data, which are mainly used for service point query and real-time query. These queries need high real-time performance. To prevent the interference of other high-load operations, the data of these two tables can be separated into a different logical cluster.
-  Tables T5 and T6 are mainly used for OLTP operations with high concurrency. Data in these tables is frequently updated and sensitive to I/O. To prevent the impact of big data query on I/O, the data of these two tables can be separated into a different logical cluster.

**Scenario 2: Isolating data for different services and enhancing the multi-tenancy of a data cluster**


.. figure:: /_static/images/en-us_image_0000002203312773.png
   :alt: **Figure 4** Logical cluster-based multi-service data and multi-tenant management

   **Figure 4** Logical cluster-based multi-service data and multi-tenant management

A large database cluster often stores data for various services. Each service has its own data tables. To allocate resources for different services, you can create multiple tenants. Specifically, assign different service users to different tenants to minimize resource contention among services. As the service scale grows continuously, the number of services in the cluster system also increases. Creating multiple tenants becomes less effective in controlling resource competition. Since each table is distributed across all DNs of a database cluster, every data table operation may involve all DNs, which increases network load and system resource consumption. Simply scaling up the cluster is not enough to solve this problem. Therefore, multiple logical clusters can be created to handle the increasing number of services, as shown in the figure above.

You can create a separate logical cluster and assign new services to it. This way, new services have little impact on existing services. Also, if the service scale in existing logical clusters grows, you can scale out the existing logical clusters.

.. note::

   A logical cluster is not suitable for managing multiple independent database systems. An independent database system requires independent O&M and needs to be managed, monitored, backed up, and upgraded separately. Moreover, faults must be isolated between clusters. Logical clusters cannot achieve independent O&M and complete fault isolation.
