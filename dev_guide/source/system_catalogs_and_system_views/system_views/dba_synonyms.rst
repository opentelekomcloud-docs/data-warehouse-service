:original_name: dws_04_0673.html

.. _dws_04_0673:

DBA_SYNONYMS
============

**DBA_SYNONYMS** displays all synonyms in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** **DBA_SYNONYMS** columns

   ================= ==== ===============================================
   Name              Type Description
   ================= ==== ===============================================
   owner             text Owner of a synonym
   schema_name       text Name of the schema to which the synonym belongs
   synonym_name      text Synonym name
   table_owner       text Owner of the associated object
   table_schema_name text Schema name of the associated object
   table_name        text Name of the associated object
   ================= ==== ===============================================
