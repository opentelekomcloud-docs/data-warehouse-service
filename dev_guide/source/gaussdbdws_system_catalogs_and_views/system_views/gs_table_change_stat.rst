:original_name: dws_04_0954.html

.. _dws_04_0954:

GS_TABLE_CHANGE_STAT
====================

**GS_TABLE_CHANGE_STAT** displays the changes of all tables (excluding foreign tables) of the database on the current node. The value of each column that indicates the number of times is the accumulated value since the instance was started.

.. table:: **Table 1** GS_TABLE_CHANGE_STAT columns

   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | Column            | Type                     | Description                                                                               |
   +===================+==========================+===========================================================================================+
   | schemaname        | Name                     | Namespace of a table                                                                      |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | relname           | Name                     | Table name                                                                                |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_vacuum       | Timestamp with time zone | Time when the last **VACUUM** operation is performed manually                             |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | vacuum_count      | Bigint                   | Number of times of manually performing the **VACUUM** operation                           |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_autovacuum   | Timestamp with time zone | Time when the last **VACUUM** operation is performed automatically                        |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | autovacuum_count  | Bigint                   | Number of times of automatically performing the **VACUUM** operation                      |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_analyze      | Timestamp with time zone | Time when the **ANALYZE** operation is performed (both manually and automatically)        |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | analyze_count     | Bigint                   | Number of times of performing the **ANALYZE** operation (both manually and automatically) |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_autoanalyze  | Timestamp with time zone | Time when the last **ANALYZE** operation is performed automatically                       |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | autoanalyze_count | Bigint                   | Number of times of automatically performing the **ANALYZE** operation                     |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | last_change       | Bigint                   | Time when the last modification (**INSERT**, **UPDATE**, or **DELETE**) is performed      |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
