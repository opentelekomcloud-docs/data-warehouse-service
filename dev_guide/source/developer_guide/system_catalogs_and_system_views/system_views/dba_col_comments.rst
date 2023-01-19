:original_name: dws_04_0660.html

.. _dws_04_0660:

DBA_COL_COMMENTS
================

**DBA_COL_COMMENTS** displays information about table colum comments in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** DBA_COL_COMMENTS columns

   =========== ===================== ===========
   Name        Type                  Description
   =========== ===================== ===========
   column_name character varying(64) Column name
   table_name  character varying(64) Table name
   owner       character varying(64) Table owner
   comments    text                  Comments
   =========== ===================== ===========
