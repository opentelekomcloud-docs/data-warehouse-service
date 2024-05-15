:original_name: dws_04_1611.html

.. _dws_04_1611:

PG_RELFILENODE_SIZE
===================

The **PG_RELFILENODE_SIZE** system catalog provides file-level space statistics. Each record in the catalog corresponds to a physical file on the disk and the size of the file.

.. table:: **Table 1** PG_RELFILENODE_SIZE columns

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                                                                                  |
   +=======================+=======================+==============================================================================================================================+
   | databaseid            | oid                   | OID of the database that the physical file belongs to If a system catalog is shared across databases, its value is **0**.    |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | tablespaceid          | oid                   | Tablespace OID of the physical file                                                                                          |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | relfilenode           | oid                   | Serial number of the physical file                                                                                           |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | backendid             | integer               | ID of the background thread that creates the physical file. Generally, the value is **-1**.                                  |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | type                  | integer               | Type of the physical file.                                                                                                   |
   |                       |                       |                                                                                                                              |
   |                       |                       | -  The value **0** indicates a data file.                                                                                    |
   |                       |                       | -  The value **1** indicates an FSM file.                                                                                    |
   |                       |                       | -  The value **2** indicates a VM file.                                                                                      |
   |                       |                       | -  The value **3** indicates a BCM file.                                                                                     |
   |                       |                       | -  If the value greater than 4 indicates the total size of the data file and BCM file of the column in a column-store table. |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | filesize              | bigint                | Size of the physical file, in bytes.                                                                                         |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
