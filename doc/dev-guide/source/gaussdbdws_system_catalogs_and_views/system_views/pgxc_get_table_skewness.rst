:original_name: dws_04_0805.html

.. _dws_04_0805:

PGXC_GET_TABLE_SKEWNESS
=======================

**PGXC_GET_TABLE_SKEWNESS** displays the data skew on tables in the current database. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view.

.. table:: **Table 1** PGXC_GET_TABLE_SKEWNESS columns

   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | Column     | Type            | Description                                                                                                                  |
   +============+=================+==============================================================================================================================+
   | schemaname | Name            | Schema name of a table                                                                                                       |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | tablename  | Name            | Name of a table                                                                                                              |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | totalsize  | Numeric         | Total size of a table, in bytes                                                                                              |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | avgsize    | numeric(1000,0) | Average table size (total table size divided by the number of DNs), which is the ideal size of tables distributed on each DN |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | maxratio   | numeric(10,3)   | Ratio of the maximum table size on a single DN to the value of **avgsize**.                                                  |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | minratio   | numeric(10,3)   | Ratio of the minimum table size on a single DN to **avgsize**                                                                |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | skewsize   | Bigint          | Table skew rate (the maximum table size on a single DN minus the minimum table size on a single DN)                          |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | skewratio  | numeric(10,3)   | Table skew rate (skewsize/avgsize)                                                                                           |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | skewstddev | numeric(1000,0) | Standard deviation of table distribution (For two tables of the same size, a larger deviation indicates a more severe skew.) |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
