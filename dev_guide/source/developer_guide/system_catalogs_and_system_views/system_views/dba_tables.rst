:original_name: dws_04_0677.html

.. _dws_04_0677:

DBA_TABLES
==========

**DBA_TABLES** displays all tables in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** DBA_TABLES columns

   +-----------------------+-----------------------+------------------------------------------------------+
   | Name                  | Type                  | Description                                          |
   +=======================+=======================+======================================================+
   | owner                 | character varying(64) | Table owner                                          |
   +-----------------------+-----------------------+------------------------------------------------------+
   | table_name            | character varying(64) | Table name                                           |
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
   | num_rows              | numeric               | The estimated number of rows in the table            |
   +-----------------------+-----------------------+------------------------------------------------------+
