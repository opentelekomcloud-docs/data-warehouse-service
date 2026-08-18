:original_name: dws_04_1211.html

.. _dws_04_1211:

Subscribing to Hybrid Data Warehouse Binlog
===========================================

Binlog Usage
------------

The HStore table within the DWS hybrid data warehouse offers binlog to facilitate the capture of database events. This enables the export of incremental data to third-party components like Flink. By consuming binlog data, you can synchronize upstream and downstream data, improving data processing efficiency.

Unlike traditional MySQL binlog, which logs all database changes and focuses on data recovery and replication. The DWS hybrid data warehouse binlog is optimized for real-time data synchronization, recording DML operations—INSERT, DELETE, and UPDATE (UPSERT)—while excluding DDL operations.

DWS Binlog has the following advantages:

-  Table-level on-demand switch: enables or disables binlog for specific tables as needed.
-  Full incremental integrated consumption: supports full synchronization followed by real-time incremental consumption after a Flink task is started.
-  Cleanup upon consumption: allows asynchronous clearing of incremental data after consumption, reducing space usage.

With Flink's real-time processing capabilities and Binlog, you can build a hybrid data warehouse efficiently without additional components like Kafka. The architecture is streamlined, and data flows efficiently, driven by Flink SQL.

Constraints and Limitations
---------------------------

#. Currently, only 8.3.0.100 and later versions support HStore and HStore Opt to record binlogs. V3 tables are in the trial commercial use phase. Before using them, contact technical support for evaluation.

#. Binlog requires a primary key, an HStore or HStore Opt table, and supports only hash distribution.

#. Binlog tables only log DML operations like INSERT, DELETE, and UPDATE (UPSERT). DDL operations are not logged.

#. Binlog tables do not support INSERT OVERWRITE, altering distribution columns, enabling Binlog on temporary tables, or partition operations like EXCHANGE, MERGE, and SPLIT PARTITION.

#. Users can perform the following DDL operations on the current binlog table. However, the incremental data and synchronization point information will be cleared.

   ADD COLUMN, DROP COLUMN, SET TYPE, and TRUNCATE

#. The system waits for binlog consumption before further scaling. The default wait time is 1 hour. Timeouts or errors will cause the scaling process to fail.

#. The system waits for the consumption of binlog records before the **VACUUM FULL** operation is performed on a binlog table. The default wait time is 1 hour. Timeouts or errors will cause the **VACUUM FULL** process to fail. Additionally, even if VACUUM FULL is executed for a partitioned table, a level-7 lock is added to the primary table of the partition, which blocks the insertion, update, or deletion of the entire table.

#. Binlog tables are backed up as standard HStore tables. Post-restoration, you must restart data synchronization as incremental data and sync details are reset.

#. The Binlog timestamp function is supported. This function can be enabled by activating :ref:`enable_binlog_timestamp <en-us_topic_0000001764650772__li125481342133319>`. Only the HStore and HStore Opt tables support this function. This constraint is supported only by clusters of version 9.1.0.200 or later.

Binlog Formats and Principles
-----------------------------

.. table:: **Table 1** binlog fields

   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Field                    | Type                  | Description                                                                                                                                                                                                                 |
   +==========================+=======================+=============================================================================================================================================================================================================================+
   | gs_binlog_sync_point     | BIGINT                | Binlog system field, which indicates the synchronization point. In common GTM mode, the value is unique and ordered.                                                                                                        |
   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | gs_binlog_event_sequence | BIGINT                | Binlog system field, which indicates the sequence of operations in the same transaction.                                                                                                                                    |
   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | gs_binlog_event_type     | CHAR                  | Binlog system field, which indicates the operation type of the current record.                                                                                                                                              |
   |                          |                       |                                                                                                                                                                                                                             |
   |                          |                       | The options are as follows:                                                                                                                                                                                                 |
   |                          |                       |                                                                                                                                                                                                                             |
   |                          |                       | -  **I** refers to INSERT, indicating that a new record is inserted into the current binlog.                                                                                                                                |
   |                          |                       | -  **d** refers to DELETE, indicating that a record is deleted from the current binlog.                                                                                                                                     |
   |                          |                       | -  **B** refers to BEFORE_UPDATE, indicating that the current binlog is a record before the update.                                                                                                                         |
   |                          |                       | -  **U** refers to AFTER_UPDATE, indicating that the current binlog is a record after the update.                                                                                                                           |
   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | gs_binlog_timestamp_us   | BIGINT                | System field of Binlog, indicating the timestamp when the current record is saved to the database.                                                                                                                          |
   |                          |                       |                                                                                                                                                                                                                             |
   |                          |                       | This field is available only when the Binlog timestamp function is enabled. If the Binlog timestamp function is disabled, this field is left blank. This field is supported only by clusters of version 9.1.0.200 or later. |
   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_column_1            | User column           | User-defined data column                                                                                                                                                                                                    |
   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ...                      | ...                   | ...                                                                                                                                                                                                                         |
   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_column_n            | User column           | User-defined data column                                                                                                                                                                                                    |
   +--------------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   -  For each UPDATE (or UPSERT-triggered update), two binlog records—BEFORE_UPDATE and AFTER_UPDATE—are created. BEFORE_UPDATE verifies the accuracy of data processed by third-party components like Flink.
   -  During UPDATE and DELETE operations, the DWS hybrid data warehouse generates BEFORE_UPDATE and DELETE binlogs without querying or populating all user columns, enhancing database import efficiency.
   -  Enabling binlog for an HStore table in the DWS hybrid data warehouse is the process of creation of a supplementary table. This table includes three system columns **gs_binlog_event_sync_point**, **gs_binlog_event_event_sequence**, and **gs_binlog_event_type**, and a **value** column that serializes all user columns.
   -  When the :ref:`enable_binlog_timestamp <en-us_topic_0000001764650772__li125481342133319>` parameter is enabled, binlog records are retained until the TTL expires, causing extra space overhead proportional to the data volume updated within the TTL. When **enable_binlog** is enabled, binlogs can be cleared asynchronously once consumed by downstream processes, significantly reducing space usage. This function is supported only by clusters of version 9.1.0.200 or later.

Enabling Binlog
---------------

-  Specify the table-level parameter **enable_binlog** when creating an HStore Opt table to enable binlogs.

   ::

      CREATE TABLE hstore_binlog_source (
          c1  INT PRIMARY KEY,
          c2  INT,
          c3  INT
      ) WITH (
          ORIENTATION = COLUMN,
          enable_hstore_opt=true,
          enable_binlog=on,
          binlog_ttl = 86400
      );

.. note::

   -  Binlog recording begins only after a synchronization point is registered for the task, not during the initial data import. Once binlog synchronization in Flink is activated, it periodically acquires the synchronization point and incremental data, then registers the synchronization point.

      -  The **binlog_ttl** parameter defaults to 86,400 seconds and is optional. If a registered synchronization point exceeds this TTL without undergoing incremental synchronization, it will be cleared. Subsequently, binlogs before the oldest synchronization point are asynchronously deleted to free up space.

   -  Space overhead: For a table with common binlog enabled, if incremental data can be consumed by downstream processes in a timely manner, the space can be cleared and reclaimed promptly.

-  Run the **ALTER** command to enable the binlog function for an existing HStore table.

   ::

      CREATE TABLE hstore_binlog_source (
          c1  INT PRIMARY KEY,
          c2  INT,
          c3  INT
      ) WITH (
          ORIENTATION = COLUMN,
          enable_hstore_opt=true
      );
      ALTER TABLE hstore_binlog_source SET (enable_binlog=on);

Querying Binlogs
----------------

-  Use the system functions provided by DWS to query the binlog information of the target table on a specified DN and check whether the binlog is consumed by downstream processes.

   ::

      -- Simulate Flink to call a system function to obtain the synchronization point. The parameters indicate the table name, slot name, whether the point is a checkpoint, and target DN (0 indicates all DNs).
      select * from pg_catalog.pgxc_get_binlog_sync_point('hstore_binlog_source', 'slot1', false, 0);
      select * from pg_catalog.pgxc_get_binlog_sync_point('hstore_binlog_source', 'slot1', true, 0);
      -- Incremental binlogs are generated after additions, deletions, and modifications.
      INSERT INTO hstore_binlog_source VALUES(100, 1, 1);
      delete hstore_binlog_source where c1 = 100;
      INSERT INTO hstore_binlog_source VALUES(200, 1, 1);
      update hstore_binlog_source set c2 =2 where c1 = 200;
      -- Simulate Flink to call a system function to query the binlog of a specified CSN range. The parameters indicate the table name, target DN (0 indicates all DNs), start CSN point, and end CSN point.
      select * from pgxc_get_binlog_changes('hstore_binlog_source', 0, 0 , 9999999999);

   |image1|

   Two **INSERT** operations generate two records with **gs_binlog_event_type** as **I**. The **DELETE** operation generates a record whose type is **d**. The **UPDATE** operation generates a **B** record for **BeforeUpdate** and a **U** record for **AfterUpdate**, indicating the values before and after the update.

-  Query the :ref:`pgxc_get_binlog_slots(rel_name text, node_id int) <en-us_topic_0000001811609965__section13183502920>` function to obtain the synchronization information of all slots of the target table. The parameters indicate the target table name and target DN (**0** indicates all DNs are queried).

   |image2|

-  Call the system function :ref:`pgxc_consumed_binlog_records <en-us_topic_0000001811609965__section156601725513>` to check whether the binlogs of the target table are consumed by all slots. The parameters indicate the target table name and target DN (**0** indicates all DNs are queried).

   ::

      -- Simulate Flink to call the system function to register a synchronization point. The parameters indicate the table name, slot name, registered point, whether the point is a checkpoint, and xmin corresponding to the point (provided when the synchronization point is obtained).
      select pgxc_register_binlog_sync_point('hstore_binlog_source', 'slot1', 0, 9999999999, false, 100);
      select pgxc_register_binlog_sync_point('hstore_binlog_source', 'slot1', 0, 9999999999, true, 100);
      -- Check whether all binlogs in the table are consumed. If 1 is returned, all binlogs have been consumed by downstream slots.
      select * from pgxc_consumed_binlog_records('hstore_binlog_source',0);

   |image3|

-  Call the system function :ref:`pgxc_binlog_clear_target_slot(rel_name text, slot_name text) <en-us_topic_0000001811609965__section115113019116>` to clear the specified slot on the specified table. The parameters indicate the target table name and target slot name, respectively.

Enabling the Binlog Timestamp Function
--------------------------------------

If you need to read binlogs generated after a specified time point, specify the table-level parameter **enable_binlog_timestamp** when creating an HStore table to enable the binlog timestamp function of the HStore table. This function is supported only by clusters of version 9.1.0.200 or later.

::

   CREATE TABLE hstore_binlog_source(
       c1  INT PRIMARY KEY,
       c2  INT,
       c3  INT
   ) WITH (
       ORIENTATION = COLUMN,
       enable_hstore_opt=true,
       enable_binlog_timestamp =on,
       binlog_ttl = 86400
   );

.. note::

   -  Binlog recording begins only after a synchronization point is registered for the task, not during the initial data import. Once the binlog timestamp is enabled, the system periodically acquires the synchronization point and incremental data, then registers the synchronization point.
   -  Binlog_ttl is an optional parameter. If not set, the default value is **86400** seconds (i.e., data is retained for one day by default). If the timestamp of the binlog record is greater than the current TTL, the binlog record will be deleted asynchronously.
   -  Space overhead: For a table with the binlog timestamp enabled, the binlog records recorded in the auxiliary table are retained until the TTL expires. This results in extra space overhead, which is proportional to the amount of data updated and imported into the database within the TTL.

-  Query the binlog on the table where binlog timestamp is enabled.

   |image4|

   Convert **gs_binlog_timestamp_us** from the BigInt type to a readable timestamp.

   ::

      SELECT to_timestamp(1731569598408661/1000000);

   |image5|

-  Obtain the first binlog information of the target table after the specified time point on each DN (if the value is empty, no binlog exists after the time point).

   ::

      SELECT * FROM pgxc_get_binlog_cursor_by_timestamp('hstore_binlog_source','2024-11-14 15:33:18.40866+08', 0);

   |image6|

-  Obtain the consumption progress of the table for which binlog timestamp is enabled.

   The returned fields indicate the timestamp of the latest consumed binlog, the latest timestamp on the binlog, the CSN point of the latest consumed binlog, the latest CSN point on the binlog, and the number of unconsumed binlog records.

   Query the consumption progress of each slot in the target table.

   ::

      SELECT * FROM pgxc_get_binlog_consume_progress('hstore_binlog_source', 0);

   |image7|

Preventing DML from Generating Binlogs
--------------------------------------

Set the session-level parameter :ref:`enable_generate_binlog <en-us_topic_0000001764491796__section192371051875>` to **off** to control the DML of the current session. When a table for which binlog is enabled is imported to the database, no binlog record is generated.

.. |image1| image:: /_static/images/en-us_image_0000002080517038.png
.. |image2| image:: /_static/images/en-us_image_0000002473386344.png
.. |image3| image:: /_static/images/en-us_image_0000002080540000.png
.. |image4| image:: /_static/images/en-us_image_0000002080725436.png
.. |image5| image:: /_static/images/en-us_image_0000002116285845.png
.. |image6| image:: /_static/images/en-us_image_0000002080726488.png
.. |image7| image:: /_static/images/en-us_image_0000002080734274.png
