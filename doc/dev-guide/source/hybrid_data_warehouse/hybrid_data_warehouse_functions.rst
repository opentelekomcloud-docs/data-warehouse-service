:original_name: dws_04_1027.html

.. _dws_04_1027:

Hybrid Data Warehouse Functions
===============================

hstore_light_merge(rel_name text)
---------------------------------

Description: This function is used to manually perform lightweight cleanup on HStore tables and holds the level-3 lock of the target table.

Return type: int

Example:

::

   SELECT hstore_light_merge('reason_select');

hstore_full_merge(rel_name text, partitionName text)
----------------------------------------------------

Description: This function is used to manually perform full cleanup on HStore tables. The second input parameter is optional and is used to specify a single partition for operations.

Return type: int

.. important::

   -  This operation forcibly merges all the visible operations of the delta table to the primary table, and then creates an empty delta table. During this period, this operation holds the level-8 lock of the table.
   -  The duration of this operation depends on the amount of data in the delta table. You must enable the HStore clearing thread to ensure unnecessary data in the HStore table is cleared in a timely manner.
   -  The second parameter **partitionName** is only supported by clusters of version 9.1.0 and later. However, these versions do not allow calling this function via **call** because it lacks reload capability.

Example:

::

   SELECT hstore_full_merge('reason_select', 'part1');

pgxc_get_small_cu_info(rel_name text, row_count int)
----------------------------------------------------

Description: Obtains the small CU information of the target table. The second parameter **row_count** is optional and indicates the small CU threshold. If the number of live tuples in a CU is fewer than the threshold, the CU is considered as a small CU. The default value is **200**. This function is supported only by clusters of version 8.2.1.200 or later.

**Return type**: record

Return value:

**node_name**: DN name.

**part_name**: partition name. This column is empty for non-partitioned tables.

**zero_cu_count**: number of 0 CUs. If all data in a CU is deleted, the CU is called 0 CU.

**small_cu_count**: number of small CUs. When a CU has live data that is less than the threshold, the CU is called a small CU.

**total_cu_count**: total number of CUs.

**sec_part_cu_num**: number of CUs in each level-2 partition. This column is displayed only when **secondary_part_column** is specified. This field is available only in clusters of version 8.3.0 or later.

It should be noted that a CU may contain multiple columns.

Example:

::

   SELECT * FROM pgxc_get_small_cu_info('hs');
   node_name | part_name | zero_cu_count | small_cu_count | total_cu_count |             sec_part_cu_num
   -----------+-----------+---------------+----------------+----------------+------------------------------------------
    datanode1 |           |             0 |              4 |              4 | p1:1 p2:0 p3:1 p4:0 p5:1 p6:0 p7:1 p8:0
    datanode2 |           |             0 |              4 |              4 | p1:0 p2:1 p3:0 p4:1 p5:0 p6:1 p7:0 p8:1
   (2 rows)

gs_hstore_compaction(rel_name text, row_count int)
--------------------------------------------------

Description: Merges small CUs of the target table. The second parameter **row_count** is optional and indicates the small CU threshold. If the number of live tuples in a CU is fewer than the threshold, the CU is considered as a small CU. The default value is **100**. This function is supported only by version 8.2.1.200 or later.

Return type: int

**Return value**: **numCompactCU**, which indicates the number of small CUs to be merged.

.. note::

   -  A CU may contain multiple columns.
   -  The partition name cannot be input in the function. Currently, a single partition cannot be specified in this function.

Example:

::

   SELECT gs_hstore_compaction('hs', 10);

pgxc_get_hstore_delta_info(rel_name text)
-----------------------------------------

Description: This function is used to obtain the delta table information of the target table, including the delta table size and the number of **INSERT**, **DELETE**, and **UPDATE** records. This function is supported only by clusters of version 8.2.1.100 or later.

Return type: record

Return value:

**node_name**: DN name.

**part_name**: partition name. This column is set to **non-partition table** if the table is not a partitioned table.

**live_tup**: number of live tuples.

**n_ui_type**: number of records with a type of *ui* (small CU combination and upsert insertion through update). An **ui** record represents a single or batch insertion. This parameter is supported only by 8.3.0.100 and later versions.

**n_i_type**: number of records whose type is **i** (insert). An **i** record indicates one insertion, which can be single insertion or batch insertion.

**n_d_type**: number of records whose type is **d** (delete). One **d** record indicates one deletion, which can be single deletion or batch deletion.

**n_x_type**: number of records whose type is **x** (deletions generated by update).

**n_u_type**: number of records whose type is **u** (lightweight update).

**n_m_type**: number of records whose type is **m** (merge).

**data_size**: total size of the **delta** table (including the size of the index and **toast** data on the **delta** table).

Example:

::

   SELECT * FROM pgxc_get_hstore_delta_info('hs_part');
    node_name | part_name | live_tup | n_ui_type | n_i_type | n_d_type | n_x_type | n_u_type | n_m_type | data_size
   -----------+-----------+----------+-----------+----------+----------+----------+----------+----------+-----------
    dn_1      | p1        |        2 |         0 |        2 |        0 |        0 |        0 |        0 |      8192
    dn_1      | p2        |        2 |         0 |        2 |        0 |        0 |        0 |        0 |      8192
    dn_1      | p3        |        2 |         0 |        2 |        0 |        0 |        0 |        0 |      8192
   (3 rows)

pgxc_get_binlog_sync_point(rel_name text, slot_name text, checkpoint bool, node_id int)
---------------------------------------------------------------------------------------

Description: Obtains the synchronization point information corresponding to a slot from the **pg_binlog_slots** system catalog. This function is applicable only to tables with binlog or binlog timestamp enabled. This function is supported only by clusters of version 9.1.0.200 or later.

**Return type**: record

Return value:

**node_name**: DN name

**node_id**: node ID

**last_sync_point**: last synchronization point

**latest_sync_point**: latest synchronization point

**xmin**: **xmin** corresponding to the synchronization point

Example:

::

   SELECT * FROM pg_catalog.pgxc_get_binlog_sync_point('hstore_binlog_source', 'slot1', false, 0);
    node_name |   node_id   | last_sync_point | latest_sync_point | xmin
   -----------+-------------+-----------------+-------------------+-------
    dn_2      | -1051926843 |               0 |             10512 | 10507
    dn_1      | -1300059100 |               0 |             10512 | 10508
   (2 rows)

pgxc_get_binlog_changes(rel_name text, node_id int, start_csn bigint, end_csn bigInt)
-------------------------------------------------------------------------------------

Description: Obtains the incremental data of the target table within the specified synchronization point range on a specified DN. If **node_id** is set to **0**, all DNs are specified. This function is applicable only to tables with binlog or binlog timestamp enabled. This function is supported only by clusters of version 9.1.0.200 or later.

**Return type**: record

Return value:

**gs_binlog_sync_point**: synchronization point

**gs_binlog_event_sequence**: sequence in the same transaction

**gs_binlog_event_type**: binlog type

**gs_binlog_timestamp_us**: timestamp of the binlog record. For the binlog table whose **enable_binlog_timestamp** is **false**, this column is empty.

**value columns**: data of each user field in the target table

Example:

::

   SELECT * FROM pgxc_get_binlog_changes('hstore_binlog_source', 0, 0 , 9999999999);
    gs_binlog_sync_point | gs_binlog_event_sequence | gs_binlog_event_type | gs_binlog_timestamp_us | c1  | c2 | c3
   ----------------------+--------------------------+----------------------+------------------------+-----+----+----
                   10516 |                        2 | I                    |       1731570520900211 | 100 |  1 |  1
                   10517 |                        3 | d                    |       1731570520904425 | 100 |  1 |  1
                   10518 |                        2 | I                    |       1731570520909055 | 200 |  1 |  1
                   10519 |                        3 | B                    |       1731570520914102 | 200 |  1 |  1
                   10519 |                        4 | U                    |       1731570520914154 | 200 |  2 |  1

pgxc_register_binlog_sync_point(rel_name text, slot_name text, node_id int, end_csn bigInt, checkpoint bool, xmin bigint)
-------------------------------------------------------------------------------------------------------------------------

Description: Registers synchronization points and can be used only for tables with binlog or binlog timestamp enabled. This function is supported only by clusters of version 9.1.0.200 or later.

Return type: int

Return value: number of nodes that are successfully registered

Example:

::

   SELECT pgxc_register_binlog_sync_point('hstore_binlog_source', 'slot1', 0, 9999999999, false, 100);
    pgxc_register_binlog_sync_point
   ---------------------------------
                                  2
   (1 row)

.. _en-us_topic_0000001811609965__section156601725513:

pgxc_consumed_binlog_records(rel_name text, node_id int)
--------------------------------------------------------

Description: Obtains the consumption status of the target table on a specified DN. This function can be used only for tables with binlog or binlog timestamp enabled. This function is supported only by clusters of version 9.1.0.200 or later.

Return type: int

Return value: If **0** is returned, the binlog of the target table is not completely consumed (including all slots and checkpoint synchronization points). If **1** is returned, the binlog of the target table is completely consumed.

Example:

::

   SELECT * FROM pgxc_consumed_binlog_records('hstore_binlog_source',0);
    pgxc_consumed_binlog_records
   ------------------------------
                               1
   (1 row)

pgxc_get_binlog_cursor_by_timestamp(rel_name text, timestamp timestampTz, node_id int)
--------------------------------------------------------------------------------------

Description: Obtains information about the first binlog record after a specified time point in the target table. This function can be used only for tables with the binlog timestamp enabled.

This function is supported only by clusters of version 9.1.0.200 or later.

**Return type**: record

Return value:

**node_name**: DN name

**node_id**: node ID

**latest_sync_point**: latest synchronization point

**binlog_sync_point**: synchronization point of the first binlog record after the time point

**binlog_timestamp_us**: timestamp of the first binlog record after the time point

**binlog_xmin**: **xmin** recorded in the first binlog after the time point

Example:

::

   SELECT * FROM pgxc_get_binlog_cursor_by_timestamp('hstore_binlog_source','2024-11-14 15:48:40.900211+08', 0);
    node_name |   node_id   | latest_sync_point | binlog_sync_point | binlog_timestamp_us | binlog_xmin
   -----------+-------------+-------------------+-------------------+---------------------+-------------
    dn_2      | -1051926843 |             10532 |             10516 |    1731570520900211 |       10510
    dn_1      | -1300059100 |             10532 |             10518 |    1731570520909055 |       10510
   (2 rows)

pgxc_get_binlog_cursor_by_syncpoint(rel_name text, csn int8, node_id int)
-------------------------------------------------------------------------

Description: Obtains the first binlog record after a specified synchronization point on the target table. This function can be used only for tables with the binlog timestamp enabled.

This function is supported only by clusters of version 9.1.0.200 or later.

**Return type**: record

Return value:

**node_name**: DN name

**node_id**: node ID

**latest_sync_point**: latest synchronization point

**binlog_sync_point**: synchronization point of the first binlog record after the time point

**binlog_timestamp_us**: timestamp of the first binlog record after the time point

**binlog_xmin**: **xmin** recorded in the first binlog after the time point

Example:

::

   SELECT * FROM pgxc_get_binlog_cursor_by_syncpoint('hstore_binlog_source',10516,0);
    node_name |   node_id   | latest_sync_point | binlog_sync_point | binlog_timestamp_us | binlog_xmin
   -----------+-------------+-------------------+-------------------+---------------------+-------------
    dn_1      | -1300059100 |             11187 |             10518 |    1731570520909055 |       10510
    dn_2      | -1051926843 |             11187 |             10516 |    1731570520900211 |       10510
   (2 rows)

pgxc_get_cstore_dirty_ratio(rel_name text, partition_name)
----------------------------------------------------------

Description: This function is used to obtain the cu, delta, and cudesc dirty page rates and sizes of the target table on each DN. Only HStore Opt tables are supported.

The **partition_name** parameter is optional. If the partition name is specified, only the information about the partition is returned. If the partition name is not specified and the table is a primary table, the information about all partitions is returned. It is supported only by clusters of version 9.1.0.100 or later.

**Return type**: record

Return value:

**node_name**: DN name

**database_name**: name of the database where the table is located

**rel_name**: primary table name

**part_name**: partition name

**cu_dirty_ratio**: dirty page rate of CU files

**cu_size**: CU file size

**delta_dirty_ratio**: dirty page rate of the delta table

**delta_size**: delta table size

**cudesc_dirty_ratio**: dirty page rate of the cudesc table

**cudesc_size**: cudesc table size

Example:

::

   SELECT * FROM pgxc_get_cstore_dirty_ratio('hs_opt_part');
    node_name | database_name |      rel_name      | partition_name | cu_dirty_ratio | cu_size | delta_dirty_ratio | delta_size | cudesc_dirty_ratio | cudesc_size
   -----------+---------------+--------------------+----------------+----------------+---------+-------------------+------------+--------------------+-------------
    dn_1      | postgres      | public.hs_opt_part | p1             |              0 |       0 |                 0 |      16384 |                  0 |       24576
    dn_1      | postgres      | public.hs_opt_part | p2             |              0 |       0 |                 0 |      16384 |                  0 |       24576
    dn_1      | postgres      | public.hs_opt_part | p3             |              0 |       0 |                 0 |      16384 |                  0 |       24576
    dn_1      | postgres      | public.hs_opt_part | p4             |              0 |       0 |                 0 |      16384 |                  0 |       24576
    dn_1      | postgres      | public.hs_opt_part | other          |              0 | 1105920 |                 0 |     524288 |                  0 |       40960
