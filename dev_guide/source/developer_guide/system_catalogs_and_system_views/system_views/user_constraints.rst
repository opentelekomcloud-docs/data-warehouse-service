:original_name: dws_04_0860.html

.. _dws_04_0860:

USER_CONSTRAINTS
================

**USER_CONSTRAINTS** displays the table constraint information accessible to the current user.

.. table:: **Table 1** USER_CONSTRAINTS columns

   +-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
   | Name                  | Type                   | Description                                                                                   |
   +=======================+========================+===============================================================================================+
   | constraint_name       | vcharacter varying(64) | Constraint name                                                                               |
   +-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
   | constraint_type       | text                   | Constraint type                                                                               |
   |                       |                        |                                                                                               |
   |                       |                        | -  **C**: Check constraint                                                                    |
   |                       |                        | -  **F**: Foreign key constraint                                                              |
   |                       |                        | -  **P**: Primary key constraint                                                              |
   |                       |                        | -  **U**: Unique constraint                                                                   |
   +-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
   | table_name            | character varying(64)  | Name of constraint-related table                                                              |
   +-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
   | index_owner           | character varying(64)  | Owner of constraint-related index (only for the unique constraint and primary key constraint) |
   +-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
   | index_name            | character varying(64)  | Name of constraint-related index (only for the unique constraint and primary key constraint)  |
   +-----------------------+------------------------+-----------------------------------------------------------------------------------------------+
