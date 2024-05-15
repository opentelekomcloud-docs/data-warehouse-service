:original_name: dws_06_0362.html

.. _dws_06_0362:

Hot and Cold Table Functions
============================

pg_obs_cold_refresh_time(table_name, time)
------------------------------------------

Description: Modifies the time when cold data in a multi-temperature table is migrated to OBS. The default value is 00:00 every day.

**table_name** indicates the name of the multi-temperature table, and the type is Name. **time** indicates the time when the data switchover task is scheduled, and the type is Time.

Return value: SUCCESS. The time is successfully modified.

Example:

::

   SELECT * FROM pg_obs_cold_refresh_time('lifecycle_table', '06:30:00');
   pg_obs_cold_refresh_time
   --------------------------
    SUCCESS
   (1 row)

pg_refresh_storage()
--------------------

Description: Switches hot data to cold data on all hot and cold tables (in OBS).

Return type: int

Fields in the returned value

-  success_count int: indicates the number of tables that are successfully switched.
-  failed_count int: indicates the number of tables that fail to be switched.

Example:

::

   SELECT * FROM pg_refresh_storage();
    success_count | failed_count
   ---------------+--------------
                1 |            0
   (1 row)

pg_lifecycle_table_data_distribute(table_name)
----------------------------------------------

Description: Views the data distribution of a cold or hot table.

**table_name** indicates the table name and cannot be left blank.

Return value: record

Example: Multiple records are generated based on the number of nodes. The following example shows the data distribution in the **w1** table when there is only one DN node.

::

   SELECT * FROM pg_catalog.pg_lifecycle_table_data_distribute('w1');
    schemaname | tablename | nodename | hotpartition | coldpartition | switchablepartition | hotdatasize | colddatasize | switchabledatasize
   ------------+-----------+----------+--------------+---------------+---------------------+-------------+--------------+--------------------
    public     | w1        | dn_1     | p2           | p1            |                     | 80 KB       | 0 bytes      | 0 bytes
   (1 row)

pg_lifecycle_node_data_distribute()
-----------------------------------

Description: Views the data distribution of all hot and cold tables.

Return value: record

Example: There are two cold and hot tables in the database. The data distribution is as follows:

::

   SELECT * FROM pg_catalog.pg_lifecycle_node_data_distribute();
    schemaname | tablename | nodename | hotpartition | coldpartition | switchablepartition | hotdatasize | colddatasize | switchabledatasize
   ------------+-----------+----------+--------------+---------------+---------------------+-------------+--------------+--------------------
    public     | w1        | dn_1     | p2           | p1            |                     |       81920 |            0 |                  0
    public     | w2        | dn_1     | p2           | p1            |                     |       81920 |            0 |                  0
   (2 rows)
