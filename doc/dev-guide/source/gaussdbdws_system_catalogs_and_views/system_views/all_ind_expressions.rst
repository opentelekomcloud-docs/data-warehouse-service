:original_name: dws_04_0646.html

.. _dws_04_0646:

ALL_IND_EXPRESSIONS
===================

**ALL_IND_EXPRESSIONS** displays information about the expression indexes accessible to the current user.

.. table:: **Table 1** ALL_IND_EXPRESSIONS columns

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
