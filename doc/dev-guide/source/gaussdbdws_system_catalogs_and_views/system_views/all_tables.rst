:original_name: dws_04_0655.html

.. _dws_04_0655:

ALL_TABLES
==========

**ALL_TABLES** displays all the tables accessible to the current user.

.. table:: **Table 1** ALL_TABLES columns

   +-----------------------+-----------------------+------------------------------------------------------+
   | Column                | Type                  | Description                                          |
   +=======================+=======================+======================================================+
   | owner                 | character varying(64) | Owner of the table                                   |
   +-----------------------+-----------------------+------------------------------------------------------+
   | table_name            | character varying(64) | Name of the table                                    |
   +-----------------------+-----------------------+------------------------------------------------------+
   | tablespace_name       | character varying(64) | Name of the tablespace that contains the table       |
   +-----------------------+-----------------------+------------------------------------------------------+
   | status                | character varying(8)  | Whether the current record is valid                  |
   +-----------------------+-----------------------+------------------------------------------------------+
   | temporary             | character(1)          | Whether the table is a temporary table               |
   |                       |                       |                                                      |
   |                       |                       | -  **Y** indicates that it is a temporary table.     |
   |                       |                       | -  **N** indicates that it is not a temporary table. |
   +-----------------------+-----------------------+------------------------------------------------------+
   | dropped               | character varying     | Whether the current record is deleted                |
   |                       |                       |                                                      |
   |                       |                       | -  **YES** indicates that it is deleted.             |
   |                       |                       | -  **NO** indicates that it is not deleted.          |
   +-----------------------+-----------------------+------------------------------------------------------+
   | num_rows              | Numeric               | Estimated number of rows in the table                |
   +-----------------------+-----------------------+------------------------------------------------------+
