:original_name: dws_04_0720.html

.. _dws_04_0720:

PG_BULKLOAD_STATISTICS
======================

On any normal node in a cluster, **PG_BULKLOAD_STATISTICS** displays the execution status of the import and export services. Each import or export service corresponds to a record. This view is accessible only to users with system administrators rights.

.. table:: **Table 1** PG_BULKLOAD_STATISTICS columns

   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name                  | Type                     | Description                                                                                                                                                      |
   +=======================+==========================+==================================================================================================================================================================+
   | node_name             | Text                     | Node name                                                                                                                                                        |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_name               | Text                     | Database name                                                                                                                                                    |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query_id              | Bigint                   | Query ID. It is equivalent to **debug_query_id**.                                                                                                                |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tid                   | Bigint                   | ID of the current thread                                                                                                                                         |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lwtid                 | Integer                  | Lightweight thread ID                                                                                                                                            |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | session_id            | Bigint                   | GDS session ID                                                                                                                                                   |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | direction             | Text                     | Service type. The options are **gds to file**, **gds from file**, **gds to pipe**, **gds from pipe**, **copy from**, and **copy to**.                            |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query                 | Text                     | Query statement                                                                                                                                                  |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | address               | Text                     | Location of the foreign table used for data import and export                                                                                                    |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query_start           | Timestamp with time zone | Start time of data import or export                                                                                                                              |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_bytes           | Bigint                   | Total size of data to be processed                                                                                                                               |
   |                       |                          |                                                                                                                                                                  |
   |                       |                          | This parameter is specified only when a GDS common file is to be imported and the record in the row comes from a CN. Otherwise, left this parameter unspecified. |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | phase                 | Text                     | Execution phase of the current service import and export. The options are **INITIALIZING**, **TRANSFER_DATA**, and **RELEASE_RESOURCE**.                         |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | done_lines            | Bigint                   | Number of lines that have been transferred                                                                                                                       |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | done_bytes            | Bigint                   | Number of bytes that have been transferred                                                                                                                       |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
