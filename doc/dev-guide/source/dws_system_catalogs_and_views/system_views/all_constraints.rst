:original_name: dws_04_0641.html

.. _dws_04_0641:

ALL_CONSTRAINTS
===============

**ALL_CONSTRAINTS** displays information about constraints accessible to the current user.

.. table:: **Table 1** ALL_CONSTRAINTS columns

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
