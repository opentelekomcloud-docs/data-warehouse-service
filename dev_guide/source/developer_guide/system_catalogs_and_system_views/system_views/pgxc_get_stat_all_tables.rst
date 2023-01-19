:original_name: dws_04_0803.html

.. _dws_04_0803:

PGXC_GET_STAT_ALL_TABLES
========================

**PGXC_GET_STAT_ALL_TABLES** displays information about insertion, update, and deletion operations on tables and the dirty page rate of tables.

Before running **VACUUM FULL** to a system catalog with a high dirty page rate, ensure that no user is performing operations it.

You are advised to run **VACUUM FULL** to tables (excluding system catalogs) whose dirty page rate exceeds 30% or run it based on service scenarios.

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
   page_dirty_rate numeric(5,2) Dirty page rate (%) of a table
   =============== ============ ==============================

GaussDB(DWS) also provides the **pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples)** and **pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples, text schema)** functions to quickly filter out tables whose dirty page rate is greater than **dirty_percent**, number of dead tuples is greater than **n_tuples**, and schema name is **schema**. For details, see "Functions and Operators > System Administration Functions > Other Functions" in the *SQL Syntax*.
