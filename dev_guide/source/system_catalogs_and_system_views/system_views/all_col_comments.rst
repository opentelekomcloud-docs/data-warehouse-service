:original_name: dws_04_0643.html

.. _dws_04_0643:

ALL_COL_COMMENTS
================

**ALL_COL_COMMENTS** displays column comments of tables and views that the current user can access.

.. table:: **Table 1** ALL_COL_COMMENTS columns

   =========== ===================== =====================
   Name        Type                  Description
   =========== ===================== =====================
   column_name character varying(64) Column name
   table_name  character varying(64) Table/View name
   owner       character varying(64) Owner of a table/view
   comments    text                  Comments
   =========== ===================== =====================
