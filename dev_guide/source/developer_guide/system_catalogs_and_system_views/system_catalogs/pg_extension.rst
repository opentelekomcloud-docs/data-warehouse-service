:original_name: dws_04_0589.html

.. _dws_04_0589:

PG_EXTENSION
============

**PG_EXTENSION** records information about the installed extensions. By default, GaussDB(DWS) has 12 extensions, that is, PLPGSQL, DIST_FDW, FILE_FDW, HDFS_FDW, HSTORE, PLDBGAPI, DIMSEARCH, PACKAGES, GC_FDW, UUID-OSSP, LOG_FDW, and ROACH_API.

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
