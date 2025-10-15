:original_name: dws_04_0959.html

.. _dws_04_0959:

PGXC_TABLE_CHANGE_STAT
======================

**PGXC_TABLE_CHANGE_STAT** displays the changes of all tables of the database on all CNs in the cluster. Except the **nodename** column of the name type added in front of each row, the names, types, and sequences of other columns are the same as those in the :ref:`GS_TABLE_CHANGE_STAT <dws_04_0954>` view.

.. table:: **Table 1** PGXC_TABLE_CHANGE_STAT columns

   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | Column            | Type                     | Description                                                                               |
   +===================+==========================+===========================================================================================+
   | nodename          | Name                     | Node name                                                                                 |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | schemaname        | Name                     | Table namespace                                                                           |
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
