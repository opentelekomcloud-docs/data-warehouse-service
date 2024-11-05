:original_name: dws_04_0916.html

.. _dws_04_0916:

Logging Content
===============

debug_print_parse
-----------------

**Parameter description**: Specifies whether to print parsing tree results.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the printing result function is enabled.
-  **off** indicates the printing result function is disabled.

**Default value**: **off**

debug_print_rewritten
---------------------

**Parameter description**: Specifies whether to print query rewriting results.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the printing result function is enabled.
-  **off** indicates the printing result function is disabled.

**Default value**: **off**

debug_print_plan
----------------

**Parameter description**: Specifies whether to print query execution results.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the printing result function is enabled.
-  **off** indicates the printing result function is disabled.

**Default value**: **off**

.. important::

   -  Debugging information about **debug_print_parse**, **debug_print_rewritten**, and **debug_print_plan** are printed only when the log level is set to **log** or higher. When these parameters are set to **on**, their debugging information will be recorded in server logs and will not be sent to client logs. You can change the log level by setting :ref:`client_min_messages <en-us_topic_0000001460882160__sbd8ad9bb6b9b48ba97f998f060dc56f3>` and :ref:`log_min_messages <en-us_topic_0000001460882160__s1ffb0797361d413d875381200fed970b>`.
   -  Do not invoke the **gs_encrypt_aes128** and **gs_decrypt_aes128** functions when **debug_print_plan** is set to **on**, preventing the risk of sensitive information disclosure. You are advised to filter parameter information of the **gs_encrypt_aes128** and **gs_decrypt_aes128** functions in the log files generated when **debug_print_plan** is set to **on**, and then provide the information to external maintenance engineers for fault locating. After you finish using the logs, delete them as soon as possible.

debug_pretty_print
------------------

**Parameter description**: Specifies the logs produced by **debug_print_parse**, **debug_print_rewritten**, and **debug_print_plan**. The output format is more readable but much longer than the output generated when this parameter is set to **off**.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the indentation is enabled.
-  **off** indicates the indentation is disabled.

**Default value**: **on**

log_duration
------------

**Parameter description**: Specifies whether to record the duration of every completed SQL statement. For clients using extended query protocols, the time required for parsing, binding, and executing steps are logged independently.

**Type**: SUSET

**Value range**: Boolean

-  If this parameter is set to **off**, the difference between setting this parameter and setting :ref:`log_min_duration_statement <en-us_topic_0000001460882160__s670cade0b3b84413bd2256e1fe1c8cdb>` is that exceeding **log_min_duration_statement** forces the text of the query to be logged, but this parameter does not.
-  If this parameter is set to **on** and **log_min_duration_statement** has a positive value, all durations are logged but the query text is included only for statements exceeding the threshold. This behavior can be used for gathering statistics in high-load situation.

**Default value**: **on**

log_error_verbosity
-------------------

**Parameter description**: Specifies the amount of detail written in the server log for each message that is logged.

**Type**: SUSET

**Value range**: enumerated values

-  **terse** indicates that the output excludes the logging of DETAIL, HINT, QUERY, and CONTEXT error information.
-  **verbose** indicates that the output includes the SQLSTATE error code, the source code file name, function name, and number of the line in which the error occurs.
-  **default** indicates that the output includes the logging of DETAIL, HINT, QUERY, and CONTEXT error information, and excludes the SQLSTATE error code, the source code file name, function name, and number of the line in which the error occurs.

**Default value**: **default**

.. _en-us_topic_0000001460882256__s80fbcd77ad5a4cdc879fe344d17b2c13:

log_lock_waits
--------------

**Parameter description**: If the time that a session used to wait a lock is longer than the value of :ref:`deadlock_timeout <en-us_topic_0000001510522321__s34083b462e2947b5a232d8b3a1465d3b>`, this parameter specifies whether to record this message in the database. This is useful in determining if lock waits are causing poor performance.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the information is recorded.
-  **off** indicates the information is not recorded.

**Default value**: **off**

.. _en-us_topic_0000001460882256__s3dd4368238fd47a2bb1de59c2142ede5:

log_statement
-------------

**Parameter description**: Specifies whether to record SQL statements. For clients using extended query protocols, logging occurs when an execute message is received, and values of the Bind parameters are included (with any embedded single quotation marks doubled).

**Type**: SUSET

.. important::

   Statements that contain simple syntax errors are not logged even if **log_statement** is set to **all**, because the log message is emitted only after basic parsing has been completed to determine the statement type. If the extended query protocol is used, this setting also does not log statements before the execution phase (during parse analysis or planning). Set **log_min_error_statement** to ERROR or lower to log such statements.

**Value range**: enumerated values

-  **none** indicates that no statement is recorded.
-  **ddl** indicates that all data definition statements, such as CREATE, ALTER, and DROP, are recorded.
-  **mod** indicates that all DDL statements and data modification statements, such as INSERT, UPDATE, DELETE, TRUNCATE, and COPY FROM, are recorded.
-  **all** indicates that all statements are recorded. The PREPARE, EXECUTE, and EXPLAIN ANALYZE statements are also recorded.

**Default value**: **none**

log_temp_files
--------------

**Parameter description**: Specifies whether to record the delete information of temporary files. Temporary files can be created for sorting, hashing, and temporary querying results. A log entry is generated for each temporary file when it is deleted.

**Type**: SUSET

**Value range**: an integer ranging from -1 to INT_MAX. The unit is KB.

-  A positive value indicates that the delete information of temporary files whose values are larger than that of **log_temp_files** is recorded.
-  If the parameter is set to **0**, all the delete information of temporary files is recorded.
-  If the parameter is set to **-1**, the delete information of no temporary files is recorded.

**Default value**: **-1**

logging_module
--------------

**Parameter description**: Specifies whether module logs can be output on the server. This parameter is a session-level parameter, and you are not advised to use the **gs_guc** tool to set it.

**Type**: USERSET

**Value range**: a string

**Default value**: **off**. All the module logs on the server can be viewed by running **show logging_module**.

**Setting method**: First, you can run **show logging_module** to view which module is controllable. For example, the query output result is as follows:

::

   show logging_module;
   logging_module
   -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   ALL,on(),off(DFS,GUC,HDFS,ORC,SLRU,MEM_CTL,AUTOVAC,ANALYZE,CACHE,ADIO,SSL,GDS,TBLSPC,WLM,SPACE,OBS,EXECUTOR,VEC_EXECUTOR,STREAM,LLVM,OPT,OPT_REWRITE,OPT_JOIN,OPT_AGG,OPT_SUBPLAN,OPT_SETOP,OPT_CARD,OPT_SKEW,SMP,UDF,COOP_ANALYZE,WLMCP,ACCELERATE,PLANHINT,PARQUET,CARBONDATA,SNAPSHOT,XACT,HANDLE,CLOG,TQUAL,EC,REMOTE,CN_RETRY,PLSQL,TEXTSEARCH,SEQ,INSTR,COMM_IPC,COMM_PARAM,CSTORE,JOB,STREAMPOOL,STREAM_CTESCAN)
   (1 row)

Controllable modules are identified by uppercase letters, and the special ID ALL is used for setting all module logs. You can control module logs to be exported by setting the log modules to **on** or **off**. Enable log output for SSL:

::

   set logging_module='on(SSL)';
   SET
   show logging_module;                                                                                                                                                                                                                                                                                                                                                                                              logging_module
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    ALL,on(SSL),off(DFS,GUC,HDFS,ORC,SLRU,MEM_CTL,AUTOVAC,ANALYZE,CACHE,ADIO,GDS,TBLSPC,WLM,SPACE,OBS,EXECUTOR,VEC_EXECUTOR,STREAM,LLVM,OPT,OPT_REWRITE,OPT_JOIN,OPT_AGG,OPT_SUBPLAN,OPT_SETOP,OPT_CARD,OPT_SKEW,SMP,UDF,COOP_ANALYZE,WLMCP,A
   CCELERATE,PLANHINT,PARQUET,CARBONDATA,SNAPSHOT,XACT,HANDLE,CLOG,TQUAL,EC,REMOTE,CN_RETRY,PLSQL,TEXTSEARCH,SEQ,INSTR,COMM_IPC,COMM_PARAM,CSTORE,JOB,STREAMPOOL,STREAM_CTESCAN)
   (1 row)

SSL log output is enabled.

The ALL identifier is equivalent to a shortcut operation. That is, logs of all modules can be enabled or disabled.

::

   set logging_module='off(ALL)';
   SET
   show logging_module;                                                                                                                                                                                                                                                                                                                                                     logging_module
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    ALL,on(),off(DFS,GUC,HDFS,ORC,SLRU,MEM_CTL,AUTOVAC,ANALYZE,CACHE,ADIO,SSL,GDS,TBLSPC,WLM,SPACE,OBS,EXECUTOR,VEC_EXECUTOR,STREAM,LLVM,OPT,OPT_REWRITE,OPT_JOIN,OPT_AGG,OPT_SUBPLAN,OPT_SETOP,OPT_CARD,OPT_SKEW,SMP,UDF,COOP_ANALYZE,WLMCP,
   ACCELERATE,PLANHINT,PARQUET,CARBONDATA,SNAPSHOT,XACT,HANDLE,CLOG,TQUAL,EC,REMOTE,CN_RETRY,PLSQL,TEXTSEARCH,SEQ,INSTR,COMM_IPC,COMM_PARAM,CSTORE,JOB,STREAMPOOL,STREAM_CTESCAN)
   (1 row)

   set logging_module='on(ALL)';
   SET
   show logging_module;                                                                                                                                                                                                                                                                                                                                  logging_module
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    ALL,on(DFS,GUC,HDFS,ORC,SLRU,MEM_CTL,AUTOVAC,ANALYZE,CACHE,ADIO,SSL,GDS,TBLSPC,WLM,SPACE,OBS,EXECUTOR,VEC_EXECUTOR,STREAM,LLVM,OPT,OPT_REWRITE,OPT_JOIN,OPT_AGG,OPT_SUBPLAN,OPT_SETOP,OPT_CARD,OPT_SKEW,SMP,UDF,COOP_ANALYZE,WLMCP,ACCELE
   RATE,PLANHINT,PARQUET,CARBONDATA,SNAPSHOT,XACT,HANDLE,CLOG,TQUAL,EC,REMOTE,CN_RETRY,PLSQL,TEXTSEARCH,SEQ,INSTR,COMM_IPC,COMM_PARAM,CSTORE,JOB,STREAMPOOL,STREAM_CTESCAN),off()
   (1 row)

COMM_IPC logs must be enabled or disabled explicitly. You can run either of the following command to enable the log function of COMM_IPC:

::

   set logging_module='on(ALL)';
   SET
   set logging_module='on(COMM_IPC)';
   SET

After the setting is performed, the log function of the COMM_IPC module will not be automatically disabled. To disable the log function of the COMM_IPC module, you must run the following commands:

::

   set logging_module='off(ALL)';
   SET
   set logging_module='off(COMM_IPC)';
   SET

**Dependency relationship**: The value of this parameter depends on the settings of :ref:`log_min_messages <en-us_topic_0000001460882160__s1ffb0797361d413d875381200fed970b>`.

enable_unshipping_log
---------------------

**Parameter description**: Specifies whether to log statements that are not pushed down. The logs help locate performance issues that may be caused by statements not pushed down.

**Type**: SUSET

**Value range**: Boolean

-  **on**: Statements not pushed down will be logged.
-  **off**: Statements not pushed down will not be logged.

**Default value**: **on**
