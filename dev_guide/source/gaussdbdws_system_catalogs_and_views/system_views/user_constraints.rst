:original_name: dws_04_0860.html

.. _dws_04_0860:

USER_CONSTRAINTS
================

**USER_CONSTRAINTS** displays the table constraint information accessible to the current user.

+-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
| Column                | Type                   | Description                                                                                   |
+=======================+========================+===============================================================================================+
| constraint_name       | vcharacter varying(64) | Constraint name                                                                               |
+-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
| constraint_type       | Text                   | Constraint type                                                                               |
|                       |                        |                                                                                               |
|                       |                        | -  **C**: Check constraint                                                                    |
|                       |                        | -  **F**: Foreign key constraint                                                              |
|                       |                        | -  **P**: Primary key constraint                                                              |
|                       |                        | -  **U**: Unique constraint.                                                                  |
+-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
| table_name            | character varying(64)  | Name of constraint-related table                                                              |
+-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
| index_owner           | character varying(64)  | Owner of constraint-related index (only for the unique constraint and primary key constraint) |
+-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
| index_name            | character varying(64)  | Name of constraint-related index (only for the unique constraint and primary key constraint)  |
+-----------------------+------------------------+-----------------------------------------------------------------------------------------------+

Example
-------

Query constraints on a specified table of the current user. Replace **t1** with the actual table name.

::

   SELECT * FROM USER_CONSTRAINTS WHERE table_name='t1';
    constraint_name | constraint_type | table_name | index_owner |  index_name
   -----------------+-----------------+------------+-------------+---------------
    c_custkey_key   | p               | t1         | u1          | c_custkey_key
   (1 row)
