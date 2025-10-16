:original_name: dws_04_0664.html

.. _dws_04_0664:

DBA_IND_EXPRESSIONS
===================

**DBA_IND_EXPRESSIONS** displays the information about expression indexes in the database. It is accessible only to users with system administrator rights.

+-------------------+-----------------------+-------------------------------------------------------+
| Column            | Type                  | Description                                           |
+===================+=======================+=======================================================+
| index_owner       | character varying(64) | Index owner                                           |
+-------------------+-----------------------+-------------------------------------------------------+
| index_name        | character varying(64) | Index name                                            |
+-------------------+-----------------------+-------------------------------------------------------+
| table_owner       | character varying(64) | Table owner                                           |
+-------------------+-----------------------+-------------------------------------------------------+
| table_name        | character varying(64) | Table name                                            |
+-------------------+-----------------------+-------------------------------------------------------+
| column_expression | Text                  | Function-based index expression of a specified column |
+-------------------+-----------------------+-------------------------------------------------------+
| column_position   | Smallint              | Position of a column in the index                     |
+-------------------+-----------------------+-------------------------------------------------------+
