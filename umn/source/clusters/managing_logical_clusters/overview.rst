:original_name: dws_01_7241.html

.. _dws_01_7241:

Overview
========

A physical cluster can be divided into logical clusters that use the node-group mechanism. Tables in a database can be allocated to different physical nodes by logical cluster. A logical cluster can contain tables from multiple databases. :ref:`Figure 1 <en-us_topic_0000001134400816__fig1216495717015>` shows the relationships between logical clusters, databases, and tables.

.. _en-us_topic_0000001134400816__fig1216495717015:

.. figure:: /_static/images/en-us_image_0000001134560832.png
   :alt: **Figure 1** Relationships between logical clusters, databases, and tables

   **Figure 1** Relationships between logical clusters, databases, and tables

.. note::

   -  Logical clusters are supported in 8.1.0.100 or later.
   -  You are advised to allocate tables in a database to the same logical cluster.

Permissions (Logical Clusters)
------------------------------

-  The **CREATE ON NODE GROUP** permission can be granted to any user or role for performing operations such as creating tables in a logical cluster.

   -  If the schema specified for a created table is a private schema of a user (that is, the schema has the same name as the user and the owner of the schema is the user), the owner of the created table defaults to the user. You do not need to associate the table with a logical cluster.
   -  If a user is associated with a logical cluster, tables are created in the cluster. Otherwise, table creation abides by the :ref:`table creation rules <en-us_topic_0000001134400816__li591882483418>` of logical clusters.
   -  Users associated with a logical cluster do not need to specify **to group** when creating a table. The associated logical cluster can be changed.

-  .. _en-us_topic_0000001134400816__li591882483418:

   Table creation rules

   -  If **to group** is not specified for a user table but **default_storage_nodegroup** is set, tables will be created in the specified logical cluster.
   -  If **default_storage_nodegroup** is set to **installation**, tables will be created in the first logical cluster, that is, the logical cluster with the smallest OID.

-  The owner of a table can be changed to any user. However, you need to check the schema and node group permissions when performing operations on the table.

-  A system administrator can be associated with a logical cluster and can create tables in multiple logical clusters.

   -  If the system administrator is associated with a logical cluster and **to group** is not specified when you create a table, the table will be created in the associated logical cluster by default. If **to group** is specified, the table is created in the specified logical cluster.
   -  If the system administrator is not associated with a logical cluster and **to group** is not specified, tables are created in the logical cluster of **default_storage_nodegroup**. For details, see the :ref:`table creation rules <en-us_topic_0000001134400816__li591882483418>`.

-  System administrator permissions can be granted to a user associated with a logical cluster, but the table creation rules also apply.
-  The logical cluster permission for accessing non-table objects (such as schemas/sequences/functions/triggers) will not be checked.
-  A resource pool must be associated with a logical cluster.

   -  A logical cluster can be associated with multiple resource pools but a resource pool can be associated with only one logical cluster.
   -  Jobs executed by logical cluster users associated with a resource pool can only use resources in the resource pool.
   -  You do not need to create a workload group to define the number of concurrent jobs in a logical cluster. Therefore, workload groups are not required for logical clusters.

-  When a logical cluster is deleted, only the table, foreign table, and resource pool objects are deleted.

   -  Objects dependent on the tables (including the partly dependent sequences/functions/triggers) in the logical cluster will also be deleted.
   -  Logical cluster associations with its users and parent-child tenants will be removed during the process. As a result, the users will be associated with the default **installation** node group and with the default global resource pool.

-  A logical cluster user can create a database if granted the permission.

Elastic Cluster
---------------

An elastic cluster consists of non-logical cluster nodes in a physical cluster in logical cluster mode. The elastic cluster is named **elastic_group**, which is a special node group that can contain multiple or no DNs.

An elastic cluster cannot be manually created. When the first logical cluster is created in a physical cluster, an elastic cluster is also automatically created and all physical nodes not belonging to the logical cluster are automatically added to the elastic cluster. DNs in the elastic cluster will be used for logical clusters created later. To create a logical cluster, ensure that your logical cluster has DNs. (DNs are not required only when you create the first logical cluster in physical cluster mode.) You can add new physical nodes to the elastic cluster through scale-out.

Replication Table Node Group
----------------------------

A replication table node group is a special node group in logical cluster mode. It can contain one or more logical clusters, but can only create replication tables. One typical scenario is to create public dimension tables. If multiple logical clusters require some common dimension tables, create a replication table node group and add the common dimension tables to it. The logical clusters contained in the replication table node group can access these dimension tables on the local DNs, with no need to access the tables on other DNs. If a logical cluster is scaled in, the replication table node group will be scaled accordingly. If the logical cluster is deleted, the replication table node group will be scaled in. However, if the replication table node group contains only one logical cluster and the logical cluster is deleted, the replication table node group will also be deleted. In this case, create tables in a logical cluster instead.

Create a replication table node group using the **CREATE NODE GROUP** SQL statement and delete one using **DROP NODE GROUP**. Before deleting a replication table node group, delete all table objects in the node group.

.. note::

   Creation of replication table node groups is supported in 8.1.2 or later.

Constraints and Limitations
---------------------------

-  The smallest unit of the creation, scale-out, and scale-in of a logical cluster is a ring. A ring consists of at least three hosts, where the primary, standby, and secondary DNs are deployed.
-  A logical cluster cannot be independently backed up or restored.
-  A logical cluster cannot be independently upgraded.
-  A logical cluster can be restarted, but cannot be independently stopped or started.
-  A physical cluster cannot be rolled back to a physical cluster after it is converted to a logical cluster.
-  Currently, logical cluster management cannot be used together with workload management.
