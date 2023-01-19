:original_name: dws_04_0863.html

.. _dws_04_0863:

USER_IND_COLUMNS
================

**USER_IND_COLUMNS** displays column information about all indexes accessible to the current user.

.. table:: **Table 1** USER_IND_COLUMNS columns

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
