:original_name: dws_04_0805.html

.. _dws_04_0805:

PGXC_GET_TABLE_SKEWNESS
=======================

**PGXC_GET_TABLE_SKEWNESS** displays the data skew on tables in the current database. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view.

.. table:: **Table 1** PGXC_GET_TABLE_SKEWNESS columns

   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | Name       | Type            | Description                                                                                                                  |
   +============+=================+==============================================================================================================================+
   | schemaname | name            | Schema name of a table                                                                                                       |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | tablename  | name            | Name of a table                                                                                                              |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | totalsize  | numeric         | Total size of a table, in bytes                                                                                              |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | avgsize    | numeric(1000,0) | Average table size (total table size divided by the number of DNs), which is the ideal size of tables distributed on each DN |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | maxratio   | numeric(10,3)   | Ratio of the maximum table size on a single DN to **avgsize**.                                                               |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | minratio   | numeric(10,3)   | Ratio of the minimum table size on a single DN to **avgsize**.                                                               |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | skewsize   | bigint          | Table skew rate (the maximum table size on a single DN minus the minimum table size on a single DN)                          |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | skewratio  | numeric(10,3)   | Table skew rate (skewsize/avgsize)                                                                                           |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | skewstddev | numeric(1000,0) | Standard deviation of table distribution (For two tables of the same size, a larger deviation indicates a more severe skew.) |
   +------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Run the following command to query the data skews of all tables in the database (the number of tables in the database is less than 10,000):

::

   SELECT * FROM pgxc_get_table_skewness ORDER BY totalsize DESC;
    schemaname |        tablename        | totalsize | avgsize | maxratio | minratio | skewsize | skewratio | skewstddev
   ------------+-------------------------+-----------+---------+----------+----------+----------+-----------+------------
    dbadmin    | reason                  |    147456 |   49152 |     .333 |     .333 |        0 |     0.000 |          0
    tpcds      | reason_t2               |     73728 |   24576 |     .556 |    0.000 |    40960 |      .556 |      21674
    dbadmin    | reason_bk               |     65536 |   21845 |     .500 |    0.000 |    32768 |      .500 |      18919
    tsearch    | pgweb                   |     49152 |   16384 |     .333 |     .333 |        0 |     0.000 |          0
    dbadmin    | student                 |     40960 |   13653 |     .400 |     .200 |     8192 |      .200 |       4730
    tsearch    | ts_zhparser             |     40960 |   13653 |     .400 |     .200 |     8192 |      .200 |       4730
    dbms_om    | gs_wlm_session_info     |     24576 |    8192 |     .333 |     .333 |        0 |     0.000 |          0
    dbms_om    | gs_wlm_ec_operator_info |     24576 |    8192 |     .333 |     .333 |        0 |     0.000 |          0
    dbms_om    | gs_wlm_operator_info    |     24576 |    8192 |     .333 |     .333 |        0 |     0.000 |          0
   (9 rows)

If the number of tables in the database is more than 10,000, do not use the **PGXC_GET_TABLE_SKEWNESS** view because it takes a long time (hours) to query the entire database for skewed columns. You are advised to refer to the definition of the **PGXC_GET_TABLE_SKEWNESS** view and use the **table_distribution()** function to define the output. This optimizes the calculation by reducing the output columns. An example is shown as follows:

::

   SELECT schemaname,tablename,max(dnsize) AS maxsize, min(dnsize) AS minsize
   FROM pg_catalog.pg_class c
   INNER JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
   INNER JOIN pg_catalog.table_distribution() s ON s.schemaname = n.nspname AND s.tablename = c.relname
   INNER JOIN pg_catalog.pgxc_class x ON c.oid = x.pcrelid AND x.pclocatortype = 'H'
   GROUP BY schemaname,tablename;
