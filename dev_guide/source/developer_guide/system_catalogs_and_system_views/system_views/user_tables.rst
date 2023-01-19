:original_name: dws_04_0877.html

.. _dws_04_0877:

USER_TABLES
===========

**USER_TABLES** displays table information in the current schema.

.. table:: **Table 1** USER_TABLES columns

   +-----------------------+-----------------------+---------------------------------------------------+
   | Name                  | Type                  | Description                                       |
   +=======================+=======================+===================================================+
   | owner                 | character varying(64) | Table owner                                       |
   +-----------------------+-----------------------+---------------------------------------------------+
   | table_name            | character varying(64) | Table name                                        |
   +-----------------------+-----------------------+---------------------------------------------------+
   | tablespace_name       | character varying(64) | Name of the tablespace where the table is located |
   +-----------------------+-----------------------+---------------------------------------------------+
   | status                | character varying(8)  | Whether the current record is valid               |
   +-----------------------+-----------------------+---------------------------------------------------+
   | temporary             | character(1)          | Whether the table is a temporary table            |
   |                       |                       |                                                   |
   |                       |                       | -  **Y**: temporary table                         |
   |                       |                       | -  **N**: not a temporary table                   |
   +-----------------------+-----------------------+---------------------------------------------------+
   | dropped               | character varying     | Whether the current record is deleted             |
   |                       |                       |                                                   |
   |                       |                       | -  **YES**: deleted                               |
   |                       |                       | -  **NO**: not deleted                            |
   +-----------------------+-----------------------+---------------------------------------------------+
   | num_rows              | numeric               | Estimated number of rows in the table             |
   +-----------------------+-----------------------+---------------------------------------------------+
