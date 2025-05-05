:original_name: dws_04_1408.html

.. _dws_04_1408:

PG_RELFILENODE_SIZE
===================

The **PG_RELFILENODE_SIZE** system catalog provides file-level space statistics. Each record in the table corresponds to a physical file on the disk and the size of the file.

.. table:: **Table 1** PG_RELFILENODE_SIZE columns

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                  |
   +=======================+=======================+==============================================================================================================================+
   | databaseid            | OID                   | OID of the database that the physical file belongs to If a system catalog is shared across databases, its value is **0**.    |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | tablespaceid          | OID                   | Tablespace OID of the physical file                                                                                          |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | relfilenode           | OID                   | Serial number of the physical file                                                                                           |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | backendid             | Integer               | ID of the background thread that creates the physical file. Generally, the value is **-1**.                                  |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | type                  | Integer               | Type of the physical file.                                                                                                   |
   |                       |                       |                                                                                                                              |
   |                       |                       | -  The value **0** indicates a data file.                                                                                    |
   |                       |                       | -  The value **1** indicates an FSM file.                                                                                    |
   |                       |                       | -  The value **2** indicates a VM file.                                                                                      |
   |                       |                       | -  The value **3** indicates a BCM file.                                                                                     |
   |                       |                       | -  If the value greater than 4 indicates the total size of the data file and BCM file of the column in a column-store table. |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
   | filesize              | Bigint                | Size of the physical file, in bytes.                                                                                         |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------+
