:original_name: dws_04_0803.html

.. _dws_04_0803:

PGXC_GET_STAT_ALL_TABLES
========================

**PGXC_GET_STAT_ALL_TABLES** displays information about insertion, update, and deletion operations on tables and the dirty page rate of tables.

Before running **VACUUM FULL** on a system catalog with a high dirty page rate, ensure that no user is performing operations on it.

You are advised to run **VACUUM FULL** on tables (excluding system catalogs) whose dirty page rate exceeds 80% or run it based on service scenarios.

.. note::

   For clusters of 8.2.0 or later, :ref:`PGXC_STAT_TABLE_DIRTY <dws_04_1046>` is recommended for querying the dirty page rate.

.. table:: **Table 1** PGXC_GET_STAT_ALL_TABLES columns

   =============== ============ ==============================
   Column          Type         Description
   =============== ============ ==============================
   relid           OID          Table OID
   relname         Name         Table name
   schemaname      Name         Schema name of the table
   n_tup_ins       Numeric      Number of inserted tuples
   n_tup_upd       Numeric      Number of updated tuples
   n_tup_del       Numeric      Number of deleted tuples
   n_live_tup      Numeric      Number of live tuples
   n_dead_tup      Numeric      Number of dead tuples
   dirty_page_rate numeric(5,2) Dirty page rate (%) of a table
   =============== ============ ==============================

For details, see "Functions and Operators" > "System Administration Functions" > "Other Functions" in the *Data Warehouse Service (DWS) SQL Syntax*.

Examples
--------

Use the view **PGXC_GET_STAT_ALL_TABLES** to query the tables whose dirty page rate is greater than 30%.

::

   SELECT * FROM PGXC_GET_STAT_ALL_TABLES WHERE dirty_page_rate>30;

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002591492636.png
