:original_name: dws_06_0347.html

.. _dws_06_0347:

Functions for Obtaining the Residual File List
==============================================

pg_get_residualfiles()
----------------------

Description: Obtains all residual file records of the current node. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

Parameter type: none

Return type: record

The following table describes return columns.

.. table:: **Table 1** pg_get_residualfiles () return fields

   ============ ==== ==================
   Column       Type Description
   ============ ==== ==================
   isverified   bool Verified or not
   isdeleted    bool Deleted or not
   dbname       text Database name
   residualfile text Data file path
   filepath     text Residual file path
   notes        text Notes
   ============ ==== ==================

Example:

::

   SELECT * FROM pg_get_residualfiles();
    isverified | isdeleted | dbname |   residualfile    |         filepath          | notes
   ------------+-----------+--------+-------------------+---------------------------+-------
    f          | f         | db2    | base/49155/114691 | pgrf_20200908160211441546 |
    f          | f         | db2    | base/49155/114694 | pgrf_20200908160211441546 |
    f          | f         | db2    | base/49155/114696 | pgrf_20200908160211441546 |
   (3 rows)

pgxc_get_residualfiles()
------------------------

Description: Unified CN query function of pg_get_residualfiles() This function is a cluster-level function and is irrelevant to the current database. It runs on CNs.

Parameter type: none

Return type: record

The following table describes return columns.

.. table:: **Table 2** pgxc_get_residualfiles () return fields

   ============ ==== ==================
   Column       Type Description
   ============ ==== ==================
   nodename     text Node name
   isverified   bool Verified or not
   isdeleted    bool Deleted or not
   dbname       text Database name
   residualfile text Data file path
   filepath     text Residual file path
   notes        text Notes
   ============ ==== ==================

Example:

::

   SELECT * FROM pgxc_get_residualfiles();
      nodename   | isverified | isdeleted |  dbname  |   residualfile    |         filepath          | notes
   --------------+------------+-----------+----------+-------------------+---------------------------+-------
    cn_5001      | f          | f         | postgres | base/15092/32803  | pgrf_20200910170129360401 |
    dn_6001_6002 | f          | f         | db2      | base/49155/114691 | pgrf_20200908160211441546 |
    dn_6001_6002 | f          | f         | db2      | base/49155/114694 | pgrf_20200908160211441546 |
    dn_6001_6002 | f          | f         | db2      | base/49155/114696 | pgrf_20200908160211441546 |
   (4 rows)
