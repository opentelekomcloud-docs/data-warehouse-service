:original_name: dws_04_0804.html

.. _dws_04_0804:

PGXC_GET_STAT_ALL_PARTITIONS
============================

**PGXC_GET_STAT_ALL_PARTITIONS** displays information about insertion, update, and deletion operations on partitions of partitioned tables and the dirty page rate of tables.

The statistics of this view depend on the **ANALYZE** operation. To obtain the most accurate information, perform the **ANALYZE** operation on the partitioned table first.

.. table:: **Table 1** PGXC_GET_STAT_ALL_PARTITIONS columns

   =============== ============ ==============================
   Column          Type         Description
   =============== ============ ==============================
   relid           oid          Table OID
   partid          oid          Partition OID
   schename        name         Schema name of a table
   relname         name         Table name
   partname        name         Partition name
   n_tup_ins       numeric      Number of inserted tuples
   n_tup_upd       numeric      Number of updated tuples
   n_tup_del       numeric      Number of deleted tuples
   n_live_tup      numeric      Number of live tuples
   n_dead_tup      numeric      Number of dead tuples
   page_dirty_rate numeric(5,2) Dirty page rate (%) of a table
   =============== ============ ==============================
