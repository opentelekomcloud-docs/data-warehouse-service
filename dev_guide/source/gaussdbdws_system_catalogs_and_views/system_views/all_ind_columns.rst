:original_name: dws_04_0645.html

.. _dws_04_0645:

ALL_IND_COLUMNS
===============

**ALL_IND_COLUMNS** displays all index columns accessible to the current user.

.. table:: **Table 1** ALL_IND_COLUMNS columns

   =============== ===================== =================================
   Column          Type                  Description
   =============== ===================== =================================
   index_owner     character varying(64) Index owner
   index_name      character varying(64) Index name
   table_owner     character varying(64) Table owner
   table_name      character varying(64) Table name
   column_name     Name                  Column name
   column_position Smallint              Position of a column in the index
   =============== ===================== =================================
