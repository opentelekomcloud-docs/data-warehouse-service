:original_name: dws_06_0349.html

.. _dws_06_0349:

Functions for Deleting Residual Files
=====================================

pg_rm_residualfiles(filepath)
-----------------------------

Description: Deletes files from a specified residual file list on the current instance. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

Parameter type: text

Return type: record

The following table describes return columns.

.. table:: **Table 1** pg_rm_residualfiles (filepath) return fields

   ====== ==== =========================
   Column Type Description
   ====== ==== =========================
   result bool Deletion completed or not
   ====== ==== =========================

Example:

::

   postgres=#SELECT * FROM pg_rm_residualfiles('pgrf_20200908160211441599');
    result
   --------
    t
   (1 row)

.. note::

   -  Residual files can be deleted only after verification using the pg_verify_residualfiles() function.
   -  All verified files, regardless which database they are in, will be deleted.
   -  If all files recorded in the specified file have been deleted, the specified file will be removed and backed up in the **$PGDATA/pg_residualfile/backup** directory.

pg_rm_residualfiles()
---------------------

Description: Deletes all files recorded on all residual file lists on the current instance. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

Parameter type: none

Return type: record

The following table describes return columns.

.. table:: **Table 2** pg_rm_residualfiles () return fields

   ======== ==== =========================
   Column   Type Description
   ======== ==== =========================
   result   bool Deletion completed or not
   filepath text Residual file path
   notes    text Notes
   ======== ==== =========================

Example:

::

   postgres=#SELECT * FROM pg_rm_residualfiles();
    result |         filepath          | notes
   --------+---------------------------+-------
    t      | pgrf_20200908160211441546 |
   (1 row)

.. note::

   -  Residual files can be deleted only after verification using the pg_verify_residualfiles() function.
   -  All verified files, regardless which database they are in, will be deleted.
   -  If all files recorded in the specified file have been deleted, the specified file will be removed and backed up in the **$PGDATA/pg_residualfile/backup** directory.

pgxc_rm_residualfiles()
-----------------------

Description: Unified CN query function of pgxc_rm_residualfiles. This function is a cluster-level function and is irrelevant to the current database. It runs on CNs.

Parameter type: none

Return type: record

The following table describes return columns.

.. table:: **Table 3** pgxc_rm_residualfiles () return fields

   ======== ==== =========================
   Column   Type Description
   ======== ==== =========================
   nodename text Node name
   result   bool Deletion completed or not
   filepath text Residual file path
   notes    text Notes
   ======== ==== =========================

Example:

::

   postgres=#SELECT * FROM pgxc_rm_residualfiles();
      nodename   | result |         filepath          | notes
   --------------+--------+---------------------------+-------
    cn_5001      | t      | pgrf_20200910170129360401 |
    dn_6001_6002 | t      | pgrf_20200908160211441546 |
   (2 rows)

pgxc_clear_disk_cache()
-----------------------

Description: This function deletes all disk cache files. This function is supported only by clusters of version 9.0.2 or later.

Return type: void

Example:

::

   SELECT pgxc_clear_disk_cache();
    pgxc_clear_disk_cache
   -----------------------

   (1 row)
