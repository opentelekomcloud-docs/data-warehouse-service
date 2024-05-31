:original_name: dws_03_2114.html

.. _dws_03_2114:

What Are the Factors Related to the Single-Table Query Performance in GaussDB(DWS)?
===================================================================================

GaussDB(DWS) uses the shared-nothing architecture, and data is stored in a distributed manner. Therefore, the distribution key, data volume, and number of partitions affect the overall query performance of a single table.

#. Distribution Key Design

   By default, GaussDB(DWS) takes the first column of the primary key as the distribution key. When you define both a primary key and a distribution key for a table, the distribution key must be a subset of the primary key. Distribution keys determine data distribution among partitions. If distribution keys are well distributed among partitions, query performance can be improved.

   If the distribution key is incorrectly selected, data skew may occur after data is imported. The usage of some disks may be much higher than that of other disks, and the cluster may become read-only in some extreme cases. Proper selection of distribution keys is critical to table query performance. In addition, proper distribution keys enable data indexes to be created and maintained more quickly.

#. Data Volume Stored in a Single Table

   The larger the amount of data stored in a single table, the poorer the query performance. If a table contains a large amount of data, you need to store the data in partitions. To convert an ordinary table to a partitioned table, you need to create a partitioned table and import data to it from the ordinary table. When you design tables, plan whether to use partitioned tables based on service requirements.

   To partition a table, comply with the following principles:

   -  Use fields with obvious ranges for partitioning, for example, date or region.
   -  The partition name must reflect the data characteristics of the partition. For example, its format can be Keyword+Range characteristics.
   -  Set the upper limit of a partition to **MAXVALUE** to prevent data overflow.

#. Number of Partitions

   Tables and indexes can be divided into smaller and easier-to-manage units. This significantly reduces search space and improves access performance.

   The number of partitions affects the query performance. If the number of partitions is too small, the query performance may deteriorate.

   GaussDB(DWS) supports range partitioning and list partitioning. In range partitioning, records are divided and inserted into multiple partitions of a table. Each partition stores data of a specific range (ranges in different partitions do not overlap). List partitioning is supported only by clusters of 8.1.3 and later versions.

When designing a data warehouse, you need to consider these factors and perform experiments to determine the optimal design scheme.
