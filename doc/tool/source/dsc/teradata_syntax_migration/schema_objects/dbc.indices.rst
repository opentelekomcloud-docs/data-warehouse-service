:original_name: dws_mt_0070.html

.. _dws_mt_0070:

DBC.INDICES
===========

DSC migrates dbc.indices to the corresponding mig_td_ext.vw_td_dbc_indices.

Example: databasename is migrated as mig_td_ext.vw_td_dbc_tables.schemaname.

**Input**

::

   sel databasename,tablename FROM dbc.indices
   WHERE tablekind='T' and trim(databasename) = '<dbname>'
   AND
   ( NOT(TRIM(tablename) LIKE ANY  (<excludelist>))
   ) AND indextype IN ( 'Q','P');

**Output**

::

   SELECT
            mig_td_ext.vw_td_dbc_indices.schemaname

   , mig_td_ext.vw_td_dbc_indices.tablename

        FROM
            mig_td_ext.vw_td_dbc_indices
        WHERE

   mig_td_ext.vw_td_dbc_indices.tablekind =  'T'

   AND TRIM(mig_td_ext.vw_td_dbc_indices.schemaname) =
   '<dbname>'

   AND( NOT( TRIM(mig_td_ext.vw_td_dbc_indices.tablename) LIKE ANY (
   ARRAY[ < excludelist > ] ) ) )
   ;

.. note::

   In dbc.indices implementation, the query should contain "AND indextype IN ( 'Q','P')". If the query does not contain "AND indextype IN ( 'Q','P')", then the query is not migrated and DSC logs the following error message:

   "Query/statement is not supported as indextype should be mentioned with values 'P' and 'Q'."

dbc.sessioninfoV
----------------

**Input**

.. code-block::

   select username,clientsystemuserid,clientipaddress,clientprogramname
     from dbc.sessioninfoV
    where sessionno = 140167641814784;

**Output**

.. code-block::

   select usename AS username, NULL::TEXT AS clientsystemuserid
     , client_addr AS clientipaddress, application_name AS clientprogramname
     from pg_catalog.pg_stat_activity
    WHERE pid = 140167641814784;

dbc.sessioninfo
---------------

**Input**

.. code-block::

   SELECT  username
   ,clientsystemuserid
    ,clientipaddress
   ,clientprogramname
    FROM
   dbc.sessioninfo
   WHERE
   sessionno = lv_mig_session ;

**Output**

.. code-block::

   select usename AS username, NULL::TEXT AS clientsystemuserid
     , client_addr AS clientipaddress, application_name AS clientprogramname
     from pg_catalog.pg_stat_activity
    WHERE pid = lv_mig_session;

Teradata "SET QUERY_BAND" with "FOR SESSION"
--------------------------------------------

**Input**

.. code-block::

   set query_band = 'AppName=${AUTO_SYS};JobName=${AUTO_JOB};TxDate=${TX_DATE};ScriptName=${script_name};' for session ;

**Output**

.. code-block::

   set query_band = 'AppName=${AUTO_SYS};JobName=${AUTO_JOB};TxDate=${TX_DATE};ScriptName=${script_name};' /* for session */;

SESSION
-------

**Input**

.. code-block::

   select Session ;
   should be migrated as below:
   SELECT pg_backend_pid();

**Output**

.. code-block::

   SELECT pg_backend_pid();
