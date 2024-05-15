:original_name: dws_04_0954.html

.. _dws_04_0954:

GS_TABLE_CHANGE_STAT
====================

**GS_TABLE_CHANGE_STAT** displays the changes of all tables (excluding foreign tables) of the database on the current node. The value of each column that indicates the number of times is the accumulated value since the instance was started.

.. table:: **Table 1** GS_TABLE_CHANGE_STAT columns

   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | Name              | Type                     | Description                                                                               |
   +===================+==========================+===========================================================================================+
   | schemaname        | name                     | Namespace of a table                                                                      |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | relname           | name                     | Table name                                                                                |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_vacuum       | timestamp with time zone | Time when the last **VACUUM** operation is performed manually                             |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | vacuum_count      | bigint                   | Number of times of manually performing the **VACUUM** operation                           |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_autovacuum   | timestamp with time zone | Time when the last **VACUUM** operation is performed automatically                        |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | autovacuum_count  | bigint                   | Number of times of automatically performing the **VACUUM** operation                      |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_analyze      | timestamp with time zone | Time when the **ANALYZE** operation is performed (both manually and automatically)        |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | analyze_count     | bigint                   | Number of times of performing the **ANALYZE** operation (both manually and automatically) |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_autoanalyze  | timestamp with time zone | Time when the last **ANALYZE** operation is performed automatically                       |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | autoanalyze_count | bigint                   | Number of times of automatically performing the **ANALYZE** operation                     |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_change       | bigint                   | Time when the last modification (**INSERT**, **UPDATE**, or **DELETE**) is performed      |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
