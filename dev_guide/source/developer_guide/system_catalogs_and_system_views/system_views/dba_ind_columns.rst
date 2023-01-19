:original_name: dws_04_0663.html

.. _dws_04_0663:

DBA_IND_COLUMNS
===============

**DBA_IND_COLUMNS** displays column information about all indexes in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** DBA_IND_COLUMNS columns

   =============== ===================== ===============================
   Name            Type                  Description
   =============== ===================== ===============================
   index_owner     character varying(64) Index owner
   index_name      character varying(64) Index name
   table_owner     character varying(64) Table owner
   table_name      character varying(64) Table name
   column_name     name                  Column name
   column_position smallint              Position of column in the index
   =============== ===================== ===============================
