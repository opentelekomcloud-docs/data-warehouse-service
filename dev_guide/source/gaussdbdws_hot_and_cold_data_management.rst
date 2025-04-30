:original_name: dws_04_0996.html

.. _dws_04_0996:

GaussDB(DWS) Hot and Cold Data Management
=========================================

Introduction to Hot and Cold Data
---------------------------------

In massive big data scenarios, as services and data volume increase, data storage and consumption increase. The need for data may vary in different time periods, therefore, data is managed in a hierarchical manner, improving data analysis performance and reducing service costs.

For example, in a network traffic analysis system, users may be interested in security events and network access in the last month, but seldom pay attention to data generated several months ago. In such scenarios, data can be classified into hot data and cold data based on time periods.

Hot and cold data is classified based on the data access frequency and update frequency.

-  Hot data: Data that is frequently accessed and updated, has a high probability of being invoked in the future, and has high requirements on access response time.
-  Cold: Data that cannot be updated or is seldom updated, seldom accessed, and has low requirements on response time.

You can define cold and hot management tables to switch cold data that meets the specified rules to OBS for storage. Cold and hot data can be automatically determined and migrated by partition.

|image1|

Hot and Cold Data Migration
---------------------------

When data is inserted to GaussDB(DWS) column-store tables, the data is first stored in hot partitions. As data accumulates, you can manually or automatically migrate the cold data to OBS for storage. The metadata, description tables, and indexes of the migrated cold data are stored locally to ensure the read performance.

Cold/Hot Switchover Policies
----------------------------

Currently, the hot and cold partitions can be switched based on LMT (Last Modify Time) and HPN (Hot Partition Number) policies. LMT indicates that the switchover is performed based on the last update time of the partition, and HPN indicates that the switchover is performed based on the number of reserved hot partitions.

-  **LMT**: Switch the hot partition data that is not updated in the last *[day]* days to the OBS tablespace as cold partition data. *[day]* is an integer ranging from 0 to 36500, in days.

   In the following figure, *day* is set to **2**, indicating that the partitions modified in the last two days are retained as the hot partitions, while the rest is retained as the cold partitions. Assume that the current time is April 30. The delete operation is performed on the partition **[4-26]** on April 30, and the insert operation is performed on the partition **[4-27]** on April 29. Therefore, partitions **[4-26][4-27][4-29][4-30]** are retained as hot partitions.

   |image2|

-  HPN: indicates the number of hot partitions to be reserved. The partitions are sequenced based on partition sequence IDs. The sequence ID of a partition is a built-in sequence number generated based on the partition boundary values and is not shown. For a range partition, a larger boundary value indicates a larger sequence ID. For a list partition, a larger maximum enumerated value of the partition boundary indicates a larger sequence ID. During the cold and hot switchover, data needs to be migrated to OBS. HPN is an integer ranging from 0 to 1600. If HPN is set to **0**, hot partitions are not reserved. During a cold/hot switchover, all partitions with data are converted to cold partitions and stored on OBS.

   In the following figure, HPN is set to 3, indicating that the last three partitions with data are retained as the hot partitions with the rest as the cold partitions during hot and cold partition switchover.

   |image3|

Hot and cold data management supports the following functions:
--------------------------------------------------------------

-  Supports DML operations on cold and hot tables, such as **INSERT**, **COPY**, **DELETE**, **UPDATE**, and **SELECT**.
-  Supports DCL operations such as permission management on cold and hot tables.
-  Supports ANALYZE, VACUUM, MERGE INTO, and PARTITION operations on cold and hot tables.
-  Supports common column-store partitioned tables to be upgraded to hot and cold data tables.
-  Supports upgrade, scale-out, scale-in, and redistribution operations on tables with cold and hot data management enabled.
-  Supports conversion between cold and hot partitions. This function is supported only in 8.3.0 or later.

Restrictions on Hot and Cold Data Management
--------------------------------------------

-  Currently, cold and hot tables support only column-store partitioned tables of version 2.0. Foreign tables do not support cold and hot partitions.
-  If you insert data into a cold partition again, the data is directly stored in OBS. It does not turn the cold table into a hot table.
-  A partition on a DN is either hot or cold. For a partition across DNs, its data on some DNs may be hot, and some may be cold.
-  If a table has both cold and hot partitions, the query becomes slow because cold data is stored on OBS and the read/write speed are lower than those of local queries.
-  Only the cold and hot switchover policies can be modified. The tablespace of cold data in cold and hot tables cannot be modified.
-  Restrictions on partitioning cold and hot tables:

   -  Data in cold partitions cannot be exchanged.
   -  **MERGE PARTITION** supports only the merge of hot-hot partitions and cold-cold partitions.
   -  Partition operations, such as **ADD**, **MERGE**, and **SPLIT**, cannot be performed on an OBS tablespace.
   -  Tablespaces of cold and hot table partitions cannot be specified or modified during table creation.

-  Cold and hot data switchover is not performed immediately upon conditions are met. Data switchover is performed only after users manually, or through a scheduler, invoke the switchover command. Currently, the automatic scheduling time is 00:00 every day and can be modified.
-  Currently, only the LMT and HPN switchover rules are supported.
-  Cold and hot data tables do not support physical fine-grained backup and restoration. Only hot data is backed up during physical backup. Cold data on OBS does not change. The backup and restoration does not support file deletion statements, such as **TRUNCATE TABLE** and **DROP TABLE**.

Examples
--------

#. Create column-store cold and hot tables and set the hot data validity period LMT to 100 days.

   ::

      CREATE TABLE lifecycle_table(i int, val text) WITH (ORIENTATION = COLUMN, storage_policy = 'LMT:100')
      PARTITION BY RANGE (i)
      (
      PARTITION P1 VALUES LESS THAN(5),
      PARTITION P2 VALUES LESS THAN(10),
      PARTITION P3 VALUES LESS THAN(15),
      PARTITION P8 VALUES LESS THAN(MAXVALUE)
      )ENABLE ROW MOVEMENT;

#. Switch cold data to the OBS tablespace.

   -  Automatic switchover: The scheduler automatically triggers the switchover at 00:00 every day.

      The automatic switchover time can be customized. For example, the time can be changed to 06:30 every morning.

      ::

         SELECT * FROM pg_obs_cold_refresh_time('lifecycle_table', '06:30:00');

   -  Manual switchover

      Perform the following operations to manually switch a single table:

      ::

         ALTER TABLE lifecycle_table refresh storage;

      Perform the following operations to switch over all cold and hot tables in batches:

      ::

         SELECT pg_catalog.pg_refresh_storage();

#. Convert cold partition data into hot partition data. This function is supported only in 8.3.0 or later.

   Convert all cold partitions to hot partitions.

   ::

      SELECT pg_catalog.reload_cold_partition('lifecycle_table');

   Convert a specified cold partition to a hot partition:

   ::

      SELECT pg_catalog.reload_cold_partition('lifecycle_table', 'cold_partition_name');

#. View data distribution in hot and cold tables.

   View the data distribution in a single table:

   ::

      SELECT * FROM pg_catalog.pg_lifecycle_table_data_distribute('lifecycle_table');

   View data distribution in all hot and cold tables.

   ::

      SELECT * FROM pg_catalog.pg_lifecycle_node_data_distribute();

.. |image1| image:: /_static/images/en-us_image_0000001925462370.png
.. |image2| image:: /_static/images/en-us_image_0000001952581373.png
.. |image3| image:: /_static/images/en-us_image_0000001925621754.png
