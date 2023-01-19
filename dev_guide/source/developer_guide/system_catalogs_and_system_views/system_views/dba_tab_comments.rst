:original_name: dws_04_0675.html

.. _dws_04_0675:

DBA_TAB_COMMENTS
================

**DBA_TAB_COMMENTS** displays comments about all tables and views in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** DBA_TAB_COMMENTS columns

   ========== ===================== ==============================
   Name       Type                  Description
   ========== ===================== ==============================
   owner      character varying(64) Owner of the table or the view
   table_name character varying(64) Name of the table or the view
   comments   text                  Comments
   ========== ===================== ==============================
