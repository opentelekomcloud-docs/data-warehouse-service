:original_name: dws_04_0589.html

.. _dws_04_0589:

PG_EXTENSION
============

**PG_EXTENSION** records information about the installed extensions. By default, GaussDB(DWS) has 15 extensions, namely, PLPGSQL, DIST_FDW, FILE_FDW, ROACH_API, HDFS_FDW, BTREE_GIN, GC_FDW, LOG_FDW, HSTORE, PACKAGES, PLDBGAPI, TSDB, DIMSEARCH, UUID-OSSP, and PGCRYPTO.

.. table:: **Table 1** PG_EXTENSION

   +----------------+---------+----------------------------------------------------------------------------+
   | Name           | Type    | Description                                                                |
   +================+=========+============================================================================+
   | extname        | name    | Extension name                                                             |
   +----------------+---------+----------------------------------------------------------------------------+
   | extowner       | oid     | Owner of the extension                                                     |
   +----------------+---------+----------------------------------------------------------------------------+
   | extnamespace   | oid     | Namespace containing the extension's exported objects                      |
   +----------------+---------+----------------------------------------------------------------------------+
   | extrelocatable | boolean | Its value is **true** if the extension can be relocated to another schema. |
   +----------------+---------+----------------------------------------------------------------------------+
   | extversion     | text    | Version number of the extension                                            |
   +----------------+---------+----------------------------------------------------------------------------+
   | extconfig      | oid[]   | Configuration information about the extension                              |
   +----------------+---------+----------------------------------------------------------------------------+
   | extcondition   | text[]  | Filter conditions for the extension's configuration information            |
   +----------------+---------+----------------------------------------------------------------------------+
