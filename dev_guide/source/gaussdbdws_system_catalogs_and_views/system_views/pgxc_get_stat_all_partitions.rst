:original_name: dws_04_0804.html

.. _dws_04_0804:

PGXC_GET_STAT_ALL_PARTITIONS
============================

**PGXC_GET_STAT_ALL_PARTITIONS** displays information about insertion, update, and deletion operations on partitions of partitioned tables and the dirty page rate of tables.

The statistics of this view depend on the **ANALYZE** operation. To obtain the most accurate information, perform the **ANALYZE** operation on the partitioned table first.

.. note::

   For clusters of 8.2.0.100 or later, 8.2.0.100 is recommended for querying the dirty page rate.

.. table:: **Table 1** PGXC_GET_STAT_ALL_PARTITIONS columns

   =============== ============ ==============================
   Name            Type         Description
   =============== ============ ==============================
   relid           oid          Table OID
   partid          oid          Partition OID
   schemaname      name         Schema name of the table
   relname         name         Table name
   partname        name         Partition name
   n_tup_ins       numeric      Number of inserted tuples
   n_tup_upd       numeric      Number of updated tuples
   n_tup_del       numeric      Number of deleted tuples
   n_live_tup      numeric      Number of live tuples
   n_dead_tup      numeric      Number of dead tuples
   page_dirty_rate numeric(5,2) Dirty page rate (%) of a table
   =============== ============ ==============================

Example
-------

Query partition tables whose dirty page rate is greater than 30%.

::

   SELECT * FROM PGXC_GET_STAT_ALL_PARTITIONS WHERE dirty_page_rate>30;
    relid | partid |   schemaname    |      relname       | partname | n_tup_ins | n_tup_upd | n_tup_del | n_live_tup | n_dead_tup | dirty_page_rate
   -------+--------+-----------------+--------------------+----------+-----------+-----------+-----------+------------+------------+-----------------
    58320 |  58626 | schema_subquery | store_hash_par     | p1       |         2 |         0 |         2 |          0 |          2 |          100.00
    58430 |  58706 | schema_subquery | store_hash_par_mor | p4       |         1 |         1 |         1 |          0 |          2 |          100.00
    58320 |  58644 | schema_subquery | store_hash_par     | p1       |         3 |         0 |         3 |          0 |          3 |          100.00
    58430 |  58770 | schema_subquery | store_hash_par_mor | p4       |         1 |         1 |         1 |          0 |          2 |          100.00
    58320 |  58643 | schema_subquery | store_hash_par     | p1       |         2 |         0 |         2 |          0 |          2 |          100.00
    58320 |  58625 | schema_subquery | store_hash_par     | p1       |         2 |         0 |         2 |          0 |          2 |          100.00
    58320 |  58579 | schema_subquery | store_hash_par     | p1       |         2 |         0 |         2 |          0 |          2 |          100.00
    58320 |  58619 | schema_subquery | store_hash_par     | p1       |         3 |         0 |         3 |          0 |          3 |          100.00
    58320 |  58627 | schema_subquery | store_hash_par     | p1       |         4 |         0 |         4 |          0 |          4 |          100.00
    58320 |  58657 | schema_subquery | store_hash_par     | p1       |         3 |         0 |         3 |          0 |          3 |          100.00
   (10 rows)
