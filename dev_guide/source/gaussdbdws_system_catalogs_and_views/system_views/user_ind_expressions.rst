:original_name: dws_04_0864.html

.. _dws_04_0864:

USER_IND_EXPRESSIONS
====================

**USER_IND_EXPRESSIONS** displays information about the function-based expression index accessible to the current user.

+-------------------+-----------------------+-----------------------------------------------------------+
| Name              | Type                  | Description                                               |
+===================+=======================+===========================================================+
| index_owner       | character varying(64) | Index owner                                               |
+-------------------+-----------------------+-----------------------------------------------------------+
| index_name        | character varying(64) | Index name                                                |
+-------------------+-----------------------+-----------------------------------------------------------+
| table_owner       | character varying(64) | Table owner                                               |
+-------------------+-----------------------+-----------------------------------------------------------+
| table_name        | character varying(64) | Table name                                                |
+-------------------+-----------------------+-----------------------------------------------------------+
| column_expression | text                  | The function-based index expression of a specified column |
+-------------------+-----------------------+-----------------------------------------------------------+
| column_position   | smallint              | Position of column in the index                           |
+-------------------+-----------------------+-----------------------------------------------------------+
