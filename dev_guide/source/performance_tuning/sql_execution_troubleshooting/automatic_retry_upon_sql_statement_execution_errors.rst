:original_name: dws_04_0497.html

.. _dws_04_0497:

Automatic Retry upon SQL Statement Execution Errors
===================================================

With automatic retry (referred to as CN retry), GaussDB(DWS) retries an SQL statement when the execution of a statement fails. If an SQL statement sent from the **gsql** client, JDBC driver, or ODBC driver fails to be executed, the CN can automatically identify the error reported during execution and re-deliver the task to retry.

The restrictions of this function are as follows:

-  Functionality restrictions:

   -  CN retry increases execution success rate but does not guarantee success.
   -  CN retry is enabled by default. In this case, the system records logs about temporary tables. If it is disabled, the system will not record the logs. Therefore, do not repeatedly enable and disable CN retry when temporary tables are used. Otherwise, data inconsistency may occur after a CN retry following a primary/standby switchover.
   -  CN retry is enabled by default. In this case, the **unlogged** keyword is ignored in the statement for creating unlogged tables and thereby ordinary tables will be created by using this statement. If CN retry is disabled, the system records logs about unlogged tables. Therefore, do not repeatedly enable and disable CN retry when unlogged tables are used. Otherwise, data inconsistency may occur after a CN retry following a primary/standby switchover.
   -  When GDS is used to export data, CN retry is supported. The existing mechanism checks for duplicate files and deletes duplicate files during data export. Therefore, you are advised not to repeatedly export data for the same foreign table unless you are sure that files with the same name in the data directory need to be deleted.

-  Error type restrictions:

   Only the error types in :ref:`Table 1 <en-us_topic_0000001578910098__en-us_topic_0000001188642200_en-us_topic_0000001098987144_table17724162161910>` are supported.

-  Statement type restrictions:

   Support single-statement CN retry, stored procedures, functions, and anonymous blocks. Statements in transaction blocks are not supported.

-  Statement restrictions of a stored procedure:

   -  If an error occurs during the execution of a stored procedure containing **EXCEPTION** (including statement block execution and statement execution in EXCEPTION), the stored procedure can be retried. If an internal error occurs, the stored procedure will retry first, but if the error is captured by **EXCEPTION**, the stored procedure cannot be retried.
   -  Packages that use global variables are not supported.
   -  **DBMS_JO** is not supported.
   -  **UTL_FILE** is not supported.
   -  If the stored procedure has printed information (such as **dbms_output.put_line** or **raise info**), the printed information will be output repeatedly when retry occurs, and "Notice: Retry triggered, some message may be duplicated. " will be output before the repeated information.

-  Cluster status restrictions:

   -  Only DNs or GTMs are faulty.
   -  The cluster can be recovered before the number of CN retries reaches the allowed maximum (controlled by **max_query_retry_times**). Otherwise, CN retry may fail.
   -  CN retry is not supported during scale-out.

-  Data import restrictions:

   -  The **COPY FROM STDIN** statement is not supported.
   -  The **gsql \\copy from** metacommand is not supported.
   -  **JDBC CopyManager copyIn** is not supported.

:ref:`Table 1 <en-us_topic_0000001578910098__en-us_topic_0000001188642200_en-us_topic_0000001098987144_table17724162161910>` lists the error types supported by CN retry and the corresponding error codes. You can use the GUC parameter **retry_ecode_list** to set the list of error types supported by CN retry. You are not advised to modify this parameter. To modify it, contact the technical support.

.. _en-us_topic_0000001578910098__en-us_topic_0000001188642200_en-us_topic_0000001098987144_table17724162161910:

.. table:: **Table 1** Error types supported by CN retry

   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Error Type                                                                | Error Code | Remarks                                                                                                                                                                                                                                        |
   +===========================================================================+============+================================================================================================================================================================================================================================================+
   | CONNECTION_RESET_BY_PEER                                                  | YY001      | TCP communication errors: Connection reset by peer (communication between the CN and DNs)                                                                                                                                                      |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | STREAM_CONNECTION_RESET_BY_PEER                                           | YY002      | TCP communication errors: Stream connection reset by peer (communication between DNs)                                                                                                                                                          |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | LOCK_WAIT_TIMEOUT                                                         | YY003      | Lock wait timeout                                                                                                                                                                                                                              |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONNECTION_TIMED_OUT                                                      | YY004      | TCP communication errors: Connection timed out                                                                                                                                                                                                 |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SET_QUERY_ERROR                                                           | YY005      | Failed to deliver the **SET** command: Set query                                                                                                                                                                                               |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | OUT_OF_LOGICAL_MEMORY                                                     | YY006      | Failed to apply for memory: Out of logical memory                                                                                                                                                                                              |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SCTP_MEMORY_ALLOC                                                         | YY007      | SCTP communication errors: Memory allocate error                                                                                                                                                                                               |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SCTP_NO_DATA_IN_BUFFER                                                    | YY008      | SCTP communication errors: SCTP no data in buffer                                                                                                                                                                                              |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SCTP_RELEASE_MEMORY_CLOSE                                                 | YY009      | SCTP communication errors: Release memory close                                                                                                                                                                                                |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SCTP_TCP_DISCONNECT                                                       | YY010      | SCTP communication errors: TCP disconnect                                                                                                                                                                                                      |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SCTP_DISCONNECT                                                           | YY011      | SCTP communication errors: SCTP disconnect                                                                                                                                                                                                     |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SCTP_REMOTE_CLOSE                                                         | YY012      | SCTP communication errors: Stream closed by remote                                                                                                                                                                                             |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SCTP_WAIT_POLL_UNKNOW                                                     | YY013      | Waiting for an unknown poll: SCTP wait poll unknown                                                                                                                                                                                            |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SNAPSHOT_INVALID                                                          | YY014      | Snapshot invalid                                                                                                                                                                                                                               |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ERRCODE_CONNECTION_RECEIVE_WRONG                                          | YY015      | Connection receive wrong                                                                                                                                                                                                                       |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | OUT_OF_MEMORY                                                             | 53200      | Out of memory                                                                                                                                                                                                                                  |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONNECTION_FAILURE                                                        | 08006      | GTM errors: Connection failure                                                                                                                                                                                                                 |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONNECTION_EXCEPTION                                                      | 08000      | Failed to communicate with DNs due to connection errors: Connection exception                                                                                                                                                                  |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ADMIN_SHUTDOWN                                                            | 57P01      | System shutdown by administrators: Admin shutdown                                                                                                                                                                                              |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | STREAM_REMOTE_CLOSE_SOCKET                                                | XX003      | Remote socket disabled: Stream remote close socket                                                                                                                                                                                             |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ERRCODE_STREAM_DUPLICATE_QUERY_ID                                         | XX009      | Duplicate query id                                                                                                                                                                                                                             |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ERRCODE_STREAM_CONCURRENT_UPDATE                                          | YY016      | Stream concurrent update                                                                                                                                                                                                                       |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ERRCODE_LLVM_BAD_ALLOC_ERROR                                              | CG003      | Memory allocation error: Allocate error                                                                                                                                                                                                        |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ERRCODE_LLVM_FATAL_ERROR                                                  | CG004      | Fatal error                                                                                                                                                                                                                                    |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | HashJoin temporary file reading error (ERRCODE_HASHJOIN_TEMP_FILE_ERROR). | F0011      | File error                                                                                                                                                                                                                                     |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Buffer file reading error (ERRCODE_BUFFER_FILE_ERROR)                     | F0012      | File reading error.                                                                                                                                                                                                                            |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Partition number error (ERRCODE_PARTITION_NUM_CHANGED).                   | 45003      | During scanning on a list partition table, it is found that the number of partitions is different from that in the optimization phase. This problem usually occurs when the queries and **ADD**/**DROP** partitions are concurrently executed. |
   +---------------------------------------------------------------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

To enable CN retry, set the following GUC parameters:

-  Mandatory GUC parameters (required by both CNs and DNs)

   max_query_retry_times

   .. caution::

      If CN retry is enabled, temporary table data is logged. For data consistency, do not switch the enabled/disabled status for CN retry when the temporary tables are being used by sessions.

-  Optional GUC parameters

   cn_send_buffer_size

   max_cn_temp_file_size
