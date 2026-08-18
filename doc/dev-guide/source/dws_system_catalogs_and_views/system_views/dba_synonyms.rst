:original_name: dws_04_0673.html

.. _dws_04_0673:

DBA_SYNONYMS
============

**DBA_SYNONYMS** displays all synonyms in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** **DBA_SYNONYMS** columns

   +-------------------+------+-----------------------------------------------------+
   | Column            | Type | Description                                         |
   +===================+======+=====================================================+
   | owner             | Text | Owner of a synonym                                  |
   +-------------------+------+-----------------------------------------------------+
   | schema_name       | Text | Name of the schema to which the synonym belongs     |
   +-------------------+------+-----------------------------------------------------+
   | synonym_name      | Text | Synonym name                                        |
   +-------------------+------+-----------------------------------------------------+
   | table_owner       | Text | Owner of the associated object                      |
   +-------------------+------+-----------------------------------------------------+
   | table_schema_name | Text | Name of the schema the associated object belongs to |
   +-------------------+------+-----------------------------------------------------+
   | table_name        | Text | Name of the associated object                       |
   +-------------------+------+-----------------------------------------------------+
