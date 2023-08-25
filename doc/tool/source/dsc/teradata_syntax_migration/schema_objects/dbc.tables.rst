:original_name: dws_mt_0069.html

.. _dws_mt_0069:

DBC.TABLES
==========

The DSC migrates dbc.tables to their corresponding mig_td_ext.vw_td_dbc_tables.

Example: databasename is migrated as mig_td_ext.vw_td_dbc_tables.schemaname.

**Input**

::

   sel databasename,tablename FROM dbc.tables
   WHERE tablekind='T' and trim(databasename) = '<dbname>'
   AND
   ( NOT(TRIM(tablename) LIKE ANY  (<excludelist>))
   );

**Output**

::

   SELECT
            mig_td_ext.vw_td_dbc_tables.schemaname
                                   , mig_td_ext.vw_td_dbc_tables.tablename

        FROM
            mig_td_ext.vw_td_dbc_tables
        WHERE
                                   mig_td_ext.vw_td_dbc_tables.tablekind =  'T'
             AND TRIM(mig_td_ext.vw_td_dbc_tables.schemaname) = '<dbname>'
             AND( NOT( TRIM(mig_td_ext.vw_td_dbc_tables.tablename) LIKE ANY ( ARRAY[ < excludelist > ] ) ) )
   ;
