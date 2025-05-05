:original_name: dws_04_0589.html

.. _dws_04_0589:

PG_EXTENSION
============

**PG_EXTENSION** records information about the installed extensions. By default, GaussDB(DWS) has 34 extensions: aio_scheduler, btree_gin, cudesckv, dimsearch, dist_fdw, functional_clog, functional_extension, functional_file, functional_hudi, functional_job, functional_largeobject, functional_memory, functional_other, functional_signal, functional_vacuum, gc_fdw, hdfs_fdw, hstore, log_fdw, operational_backup, operational_cgroup, operational_cudesc, operational_other, operational_replication, operational_restoration, operational_stats, operational_xlog, packages, pgcrypto, pldbgapi, plpgsql, roach_api, tsdb, and uuid-ossp.

.. table:: **Table 1** PG_EXTENSION

   +----------------+---------+-----------------------------------------------------------------+
   | Column         | Type    | Description                                                     |
   +================+=========+=================================================================+
   | extname        | Name    | Extension name                                                  |
   +----------------+---------+-----------------------------------------------------------------+
   | extowner       | OID     | Owner of the extension                                          |
   +----------------+---------+-----------------------------------------------------------------+
   | extnamespace   | OID     | Namespace containing the extension's exported objects           |
   +----------------+---------+-----------------------------------------------------------------+
   | extrelocatable | boolean | Whether the extension can be relocated to another schema        |
   +----------------+---------+-----------------------------------------------------------------+
   | extversion     | Text    | Version number of the extension                                 |
   +----------------+---------+-----------------------------------------------------------------+
   | extconfig      | oid[]   | Configuration information about the extension                   |
   +----------------+---------+-----------------------------------------------------------------+
   | extcondition   | Text[]  | Filter conditions for the extension's configuration information |
   +----------------+---------+-----------------------------------------------------------------+
