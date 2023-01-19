:original_name: dws_04_0079.html

.. _dws_04_0079:

Table Design
============

GaussDB(DWS) uses a distributed architecture. Data is distributed on DNs. Comply with the following principles to properly design a table:

-  [Notice] Evenly distribute data on each DN to prevent data skew. If most data is stored on several DNs, the effective capacity of a cluster decreases. Select a proper distribution column to avoid data skew.
-  [Notice] Evenly scan each DN when querying tables. Otherwise, DNs most frequently scanned will become the performance bottleneck. For example, when you use equivalent filter conditions on a fact table, the nodes are not evenly scanned.
-  [Notice] Reduce the amount of data to be scanned. You can use the pruning mechanism of a partitioned table.
-  [Notice] Minimize random I/O. By clustering or local clustering, you can sequentially store hot data, converting random I/O to sequential I/O to reduce the cost of I/O scanning.
-  [Notice] Try to avoid data shuffling. To shuffle data is to physically transfer it from one node to another. This unnecessarily occupies many network resources. To reduce network pressure, locally process data, and to improve cluster performance and concurrency, you can minimize data shuffling by using proper association and grouping conditions.

Selecting a Storage Mode
------------------------

[Proposal] Selecting a storage mode is the first step in defining a table. The storage mode mainly depends on the customer's service type. For details, see :ref:`Table 1 <en-us_topic_0000001145694995__table3891877>`.

.. _en-us_topic_0000001145694995__table3891877:

.. table:: **Table 1** Table storage modes and scenarios

   +-----------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Storage Mode                      | Application Scenarios                                                                                       |
   +===================================+=============================================================================================================+
   | Row storage                       | -  Point queries (simple index-based queries that only return a few records)                                |
   |                                   | -  Scenarios requiring frequent addition, deletion, and modification                                        |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Column storage                    | -  Statistical analysis queries (requiring a large number of association and grouping operations)           |
   |                                   | -  Ad hoc queries (using uncertain query conditions and unable to utilize indexes to scan row-store tables) |
   +-----------------------------------+-------------------------------------------------------------------------------------------------------------+

Selecting a Distribution Mode
-----------------------------

[Proposal] Comply with the following rules to distribute table data.

.. table:: **Table 2** Table distribution modes and scenarios

   +-------------------+------------------------------------------------------------+--------------------------------------------------------------------+
   | Distribution Mode | Description                                                | Application Scenarios                                              |
   +===================+============================================================+====================================================================+
   | Hash              | Table data is distributed on all DNs in a cluster by hash. | Fact tables containing a large amount of data                      |
   +-------------------+------------------------------------------------------------+--------------------------------------------------------------------+
   | Replication       | Full data in a table is stored on every DN in a cluster.   | Dimension tables and fact tables containing a small amount of data |
   +-------------------+------------------------------------------------------------+--------------------------------------------------------------------+

Selecting a Partitioning Mode
-----------------------------

Comply with the following rules to partition a table containing a large amount of data:

-  [Proposal] Create partitions on columns that indicate certain ranges, such as dates and regions.
-  [Proposal] A partition name should show the data characteristics of a partition. For example, its format can be Keyword+Range characteristics.
-  [Proposal] Set the upper limit of a partition to **MAXVALUE** to prevent data overflow.

The example of a partitioned table definition is as follows:

::

   CREATE TABLE staffS_p1
   (
     staff_ID       NUMBER(6) not null,
     FIRST_NAME     VARCHAR2(20),
     LAST_NAME      VARCHAR2(25),
     EMAIL          VARCHAR2(25),
     PHONE_NUMBER   VARCHAR2(20),
     HIRE_DATE      DATE,
     employment_ID  VARCHAR2(10),
     SALARY         NUMBER(8,2),
     COMMISSION_PCT NUMBER(4,2),
     MANAGER_ID     NUMBER(6),
     section_ID     NUMBER(4)
   )
   PARTITION BY RANGE (HIRE_DATE)
   (
      PARTITION HIRE_19950501 VALUES LESS THAN ('1995-05-01 00:00:00'),
      PARTITION HIRE_19950502 VALUES LESS THAN ('1995-05-02 00:00:00'),
      PARTITION HIRE_maxvalue VALUES LESS THAN (MAXVALUE)
   );

Selecting a Distribution Key
----------------------------

Selecting a distribution key is important for a hash table. An improper distribution key may cause data skew. As a result, the I/O load is heavy on several DNs, affecting the overall query performance. After you select a distribution policy for a hash table, check for data skew to ensure that data is evenly distributed. Comply with the following rules to select a distribution key:

-  [Proposal] Select a column containing discrete data as the distribution key, so that data can be evenly distributed on each DN. If a single column is not discrete enough, consider using multiple columns as distribution keys. You can select the primary key of a table as the distribution key. For example, in an employee information table, select the certificate number column as the distribution key.
-  [Proposal] If the first rule is met, do not select a column having constant filter conditions as the distribution key. For example, in a query on the **dwcjk** table, if the **zqdh** column contains the constant filter condition **zqdh='000001'**, avoid selecting the **zqdh** column as the distribution key.
-  [Proposal] If the first and second rules are met, select the join conditions in a query as distribution keys. If a join condition is used as a distribution key, the data involved in a join task is locally distributed on DNs, which greatly reduces the data flow cost among DNs.
