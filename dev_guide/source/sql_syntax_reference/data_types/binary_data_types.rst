:original_name: dws_06_0013.html

.. _dws_06_0013:

Binary Data Types
=================

:ref:`Table 1 <en-us_topic_0000001811515497__teea287ea2f5d4444bb3ebb521189b9e8>` lists the binary data types that can be used in GaussDB(DWS).

.. _en-us_topic_0000001811515497__teea287ea2f5d4444bb3ebb521189b9e8:

.. table:: **Table 1** Binary Data Types

   +-----------------------+-------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | Name                  | Description                                                             | Storage Space                                                                                           |
   +=======================+=========================================================================+=========================================================================================================+
   | BLOB                  | Binary large object.                                                    | The maximum size is 10,7373,3621 bytes (1 GB - 8203 bytes).                                             |
   |                       |                                                                         |                                                                                                         |
   |                       | Currently, BLOB only supports the following external access interfaces: |                                                                                                         |
   |                       |                                                                         |                                                                                                         |
   |                       | -  DBMS_LOB.GETLENGTH                                                   |                                                                                                         |
   |                       | -  DBMS_LOB.READ                                                        |                                                                                                         |
   |                       | -  DBMS_LOB.WRITE                                                       |                                                                                                         |
   |                       | -  DBMS_LOB.WRITEAPPEND                                                 |                                                                                                         |
   |                       | -  DBMS_LOB.COPY                                                        |                                                                                                         |
   |                       | -  DBMS_LOB.ERASE                                                       |                                                                                                         |
   |                       |                                                                         |                                                                                                         |
   |                       | For details about the interfaces, see DBMS_LOB.                         |                                                                                                         |
   |                       |                                                                         |                                                                                                         |
   |                       | .. note::                                                               |                                                                                                         |
   |                       |                                                                         |                                                                                                         |
   |                       |    Column storage cannot be used for the BLOB type.                     |                                                                                                         |
   +-----------------------+-------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | RAW                   | Variable-length hexadecimal string                                      | 4 bytes plus the actual hexadecimal string. The maximum size is 10,7373,3621 bytes (1 GB - 8203 bytes). |
   |                       |                                                                         |                                                                                                         |
   |                       | .. note::                                                               |                                                                                                         |
   |                       |                                                                         |                                                                                                         |
   |                       |    Column storage cannot be used for the raw type.                      |                                                                                                         |
   +-----------------------+-------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | BYTEA                 | Variable-length binary string                                           | 4 bytes plus the actual binary string. The maximum size is 10,7373,3621 bytes (1 GB - 8203 bytes).      |
   +-----------------------+-------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+

.. note::

   In addition to the size limitation on each column, the total size of each tuple is 8203 bytes less than 1 GB.

Examples

::

   -- Create a table:
   CREATE TABLE blob_type_t1
   (
       BT_COL1 INTEGER,
       BT_COL2 BLOB,
       BT_COL3 RAW,
       BT_COL4 BYTEA
   ) DISTRIBUTE BY REPLICATION;

   --Insert data:
   INSERT INTO blob_type_t1 VALUES(10,empty_blob(),
   HEXTORAW('DEADBEEF'),E'\\xDEADBEEF');

   -- Query data in the table:
   SELECT * FROM blob_type_t1;
    bt_col1 | bt_col2 | bt_col3  |  bt_col4
   ---------+---------+----------+------------
         10 |         | DEADBEEF | \xdeadbeef
   (1 row)

   -- Delete the tables:
   DROP TABLE blob_type_t1;
