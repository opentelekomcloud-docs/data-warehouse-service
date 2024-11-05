:original_name: dws_04_0229.html

.. _dws_04_0229:

Analyzing a Table
=================

The execution plan generator needs to use table statistics to generate the most effective query execution plan to improve query performance. After data is imported, you are advised to run the **ANALYZE** statement to update table statistics. The statistics are stored in the **PG_STATISTIC** system catalog.


Analyzing a Table
-----------------

**ANALYZE** supports row-store, column-store, HDFS, and OBS tables in ORC or CARBONDATA format. **ANALYZE** can also collect statistics about specified columns of a local table.

Do **ANALYZE** to the **product_info** table.

::

   ANALYZE product_info;

Automatically Analyzing a Table
-------------------------------

GaussDB(DWS) provides automatic table analysis for the following two scenarios.

-  If **ANALYZE** is triggered because a query contains a table that has no statistics or a table whose amount of data modification reaches the threshold, and the execution plan does not use Fast Query Shipping (FQS), the GUC parameter :ref:`autoanalyze <en-us_topic_0000001460562696__section114241119217>` is used to control the automatic collection of table statistics. In this case, a better execution plan is generated based on the collected statistics.
-  If :ref:`autovacuum <en-us_topic_0000001510283565__s8d6c38309e594a16a07f79ae412b63c6>` is set to **on**, the system periodically starts the autovacuum thread and automatically collects statistics on the tables whose amount of data modification reaches the threshold for triggering **ANALYZE** in the background.

   .. table:: **Table 1** Automatically Analyzing a Table

      +------------------+----------------------------------------------------------------+---------------------------------+-------------------------------------+----------------------------------------------------------------------------+
      | Trigger Mode     | Trigger Condition                                              | Scaling Frequency               | Control Parameter                   | Remarks                                                                    |
      +==================+================================================================+=================================+=====================================+============================================================================+
      | Synchronization  | Statistics are completely missing.                             | At each query execution         | autoanalyze                         | Statistics are cleared when the primary table is truncated.                |
      +------------------+----------------------------------------------------------------+---------------------------------+-------------------------------------+----------------------------------------------------------------------------+
      | Synchronization  | The amount of modified data reaches the **ANALYZE** threshold. | At each query execution         | autoanalyze                         | ANALYZE is triggered before the optimal plan is determined.                |
      +------------------+----------------------------------------------------------------+---------------------------------+-------------------------------------+----------------------------------------------------------------------------+
      | Asynchronization | The amount of modified data reaches the **ANALYZE** threshold. | Autovacuum thread polling check | autovacuum_mode, autovacuum_naptime | The lock times out in 2 seconds, and the execution times out in 5 minutes. |
      +------------------+----------------------------------------------------------------+---------------------------------+-------------------------------------+----------------------------------------------------------------------------+

.. important::

   -  The autoanalyze function supports only the default sampling mode and not the percentage sampling mode.
   -  The autoanalyze function does not collect multi-column statistics, which only supports percentage sampling.
   -  **AUTOANALYZE** is triggered because a query contains a table that has no statistics or a table whose amount of data modification reaches the threshold. In this case, **AUTOANALYZE** cannot be triggered for foreign tables or temporary tables with the **ON COMMIT [DELETE ROWS \| DROP]** option.
   -  If the amount of data modification reaches the threshold for triggering **ANALYZE**, the amount of data modification exceeds **autovacuum_analyze_threshold** + **autovacuum_analyze_scale_factor \*** *reltuples*. *reltuples* indicates the estimated number of rows in the table recorded in **pg_class**.
   -  The autoanalyze function triggered by a scheduled **autovacuum** thread supports only row-store and column-store tables. It does not support foreign tables, HDFS tables, OBS foreign tables, temporary tables, unlogged tables, or toast tables.
   -  When **ANALYZE** is triggered during a query, a level-4 lock is added to all partitions in the partitioned table. The lock is released only after the transaction containing the query is committed. The level-4 lock does not block adding, deletion, modification, and query operations, but blocks partition modification operations such as **TRUNCATE**. You can set **object_mtime_record_mode** to **disable_partition** to release the partition locks in advance.
   -  The autovacuum function also depends on the following two GUC parameters in addition to **autovacuum**:

      -  :ref:`track_counts <en-us_topic_0000001510402293__s4682d08468f84845bfdc6ae9477126e8>` must be set to **on** to enable statistics collection about the database.
      -  :ref:`autovacuum_max_workers <en-us_topic_0000001510283565__s502d4304994d4da5bd3cda661aab27ac>` must be set to a value greater than 0 to specify the maximum number of concurrent autovacuum threads.
