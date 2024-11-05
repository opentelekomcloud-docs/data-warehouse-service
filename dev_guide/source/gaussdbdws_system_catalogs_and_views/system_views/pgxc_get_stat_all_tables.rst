:original_name: dws_04_0803.html

.. _dws_04_0803:

PGXC_GET_STAT_ALL_TABLES
========================

**PGXC_GET_STAT_ALL_TABLES** displays information about insertion, update, and deletion operations on tables and the dirty page rate of tables.

Before running **VACUUM FULL** on a system catalog with a high dirty page rate, ensure that no user is performing operations on it.

You are advised to run **VACUUM FULL** to tables (excluding system catalogs) whose dirty page rate exceeds 80% or run it based on service scenarios.

.. note::

   For clusters of 8.2.0.100 or later, 8.2.0.100 is recommended for querying the dirty page rate.

.. table:: **Table 1** PGXC_GET_STAT_ALL_TABLES columns

   =============== ============ ==============================
   Name            Type         Description
   =============== ============ ==============================
   relid           oid          Table OID
   relname         name         Table name
   schemaname      name         Schema name of the table
   n_tup_ins       numeric      Number of inserted tuples
   n_tup_upd       numeric      Number of updated tuples
   n_tup_del       numeric      Number of deleted tuples
   n_live_tup      numeric      Number of live tuples
   n_dead_tup      numeric      Number of dead tuples
   dirty_page_rate numeric(5,2) Dirty page rate (%) of a table
   =============== ============ ==============================

Examples
--------

Use the view **PGXC_GET_STAT_ALL_TABLES** to query the tables whose dirty page rate is greater than 30%.

::

   SELECT * FROM PGXC_GET_STAT_ALL_TABLES WHERE dirty_page_rate>30;
    relid |         relname         | schemaname | n_tup_ins | n_tup_upd | n_tup_del | n_live_tup | n_dead_tup | dirty_page_rate
   -------+-------------------------+------------+-----------+-----------+-----------+------------+------------+-----------------
     2840 | pg_toast_2619           | pg_toast   |      7415 |         0 |      7415 |          0 |        291 |           88.00
     9001 | pgxc_class              | pg_catalog |     56331 |         3 |     56285 |         54 |        143 |           72.59
    53860 | reason                  | dbadmin    |         9 |        19 |         0 |          9 |         19 |           67.86
     9025 | pg_object               | pg_catalog |    112858 |   1179707 |    112619 |        246 |        429 |           63.56
     9015 | pgxc_node               | pg_catalog |        15 |        24 |         0 |         15 |         24 |           61.54
     2606 | pg_constraint           | pg_catalog |        78 |         0 |        42 |         36 |         42 |           53.85
     1260 | pg_authid               | pg_catalog |         6 |         6 |         0 |          6 |          6 |           50.00
   (7 rows)
