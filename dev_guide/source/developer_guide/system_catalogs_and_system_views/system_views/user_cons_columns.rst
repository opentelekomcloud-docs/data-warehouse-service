:original_name: dws_04_0861.html

.. _dws_04_0861:

USER_CONS_COLUMNS
=================

**USER_CONSTRAINTS** displays the information about constraint columns of the tables accessible to the current user.

.. table:: **Table 1** USER_CONS_COLUMNS columns

   +-----------------+-----------------------+-------------------------------------+
   | Name            | Type                  | Description                         |
   +=================+=======================+=====================================+
   | table_name      | character varying(64) | Name of constraint-related table    |
   +-----------------+-----------------------+-------------------------------------+
   | column_name     | character varying(64) | Name of constraint-related column   |
   +-----------------+-----------------------+-------------------------------------+
   | constraint_name | character varying(64) | Constraint name                     |
   +-----------------+-----------------------+-------------------------------------+
   | position        | smallint              | Position of the column in the table |
   +-----------------+-----------------------+-------------------------------------+
