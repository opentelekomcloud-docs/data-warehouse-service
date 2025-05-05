:original_name: dws_04_0915.html

.. _dws_04_0915:

Logging Time
============

.. _en-us_topic_0000001811490985__sbd8ad9bb6b9b48ba97f998f060dc56f3:

client_min_messages
-------------------

**Parameter description**: Specifies which level of messages are sent to the client. Each level covers all the levels following it. The lower the level is, the fewer messages are sent.

**Type**: USERSET

.. important::

   When the values of **client_min_messages** and :ref:`log_min_messages <en-us_topic_0000001811490985__s1ffb0797361d413d875381200fed970b>` are the same, the levels are different.

**Valid values**: Enumerated values. Valid values: **debug5**, **debug4**, **debug3**, **debug2**, **debug1**, **info**, **log**, **notice**, **warning**, **error** For details about the parameters, see :ref:`Table 1 <en-us_topic_0000001811490985__ta1ceaf79532d439ead5e098dee6f7b7e>`.

**Default value:** **notice**

.. _en-us_topic_0000001811490985__s1ffb0797361d413d875381200fed970b:

log_min_messages
----------------

**Parameter description**: Specifies which level of messages will be written into server logs. Each level covers all the levels following it. The lower the level is, the fewer messages will be written into the log.

**Type**: SUSET

.. important::

   When the values of :ref:`client_min_messages <en-us_topic_0000001811490985__sbd8ad9bb6b9b48ba97f998f060dc56f3>` and **log_min_messages** are the same, the levels are different.

**Value range**: enumerated type. Valid values: **debug5**, **debug4**, **debug3**, **debug2**, **debug1**, **info**, **log**, **notice**, **warning**, **error**, **fatal**, **panic** For details about the parameters, see :ref:`Table 1 <en-us_topic_0000001811490985__ta1ceaf79532d439ead5e098dee6f7b7e>`.

**Default value**: **warning**

log_min_error_statement
-----------------------

**Parameter description**: Specifies which SQL statements that cause errors condition will be recorded in the server log.

**Type**: SUSET

**Value range**: enumerated type. Valid values: **debug5**, **debug4**, **debug3**, **debug2**, **debug1**, **info**, **log**, **notice**, **warning**, **error**, **fatal**, **panic** For details about the parameters, see :ref:`Table 1 <en-us_topic_0000001811490985__ta1ceaf79532d439ead5e098dee6f7b7e>`.

.. note::

   -  The default is **error**, indicating that statements causing errors, log messages, fatal errors, or panics will be logged.
   -  **panic**: This feature is disabled.

**Default value**: **error**

.. _en-us_topic_0000001811490985__s670cade0b3b84413bd2256e1fe1c8cdb:

log_min_duration_statement
--------------------------

**Parameter description**: Specifies the threshold for logging statement execution durations. The execution duration that is greater than the specified value will be logged.

This parameter helps track query statements that need to be optimized. For clients using extended query protocol, durations of the Parse, Bind, and Execute are logged independently.

**Type**: SUSET

.. important::

   If this parameter and :ref:`log_statement <en-us_topic_0000001811491197__s3dd4368238fd47a2bb1de59c2142ede5>` are used at the same time, statements recorded based on the value of **log_statement** will not be logged again after their execution duration exceeds the value of this parameter. If you are not using **syslog**, it is recommended that you log the process ID (PID) or session ID using log_line_prefix so that you can link the current statement message to the last logged duration.

**Value range**: an integer ranging from -1 to INT_MAX. The unit is millisecond.

-  If this parameter is set to **250**, execution durations of SQL statements that run 250 ms or longer will be logged.
-  **0**: Execution durations of all the statements are logged.
-  **-1**: This feature is disabled.

**Default value**: **30min**

backtrace_min_messages
----------------------

**Parameter description**: Prints the function's stack information to the server's log file if the level of information generated is greater than or equal to this parameter level.

**Type**: SUSET

.. important::

   This parameter is used for locating customer on-site problems. Because frequent stack printing will affect the system's overhead and stability, therefore, when you locate the onsite problems, set the value of this parameter to ranks other than **fatal** and **panic**.

**Value range**: enumerated values

Valid values: **debug5**, **debug4**, **debug3**, **debug2**, **debug1**, **info**, **log**, **notice**, **warning**, **error**, **fatal**, **panic** For details about the parameters, see :ref:`Table 1 <en-us_topic_0000001811490985__ta1ceaf79532d439ead5e098dee6f7b7e>`.

**Default value**: **panic**

:ref:`Table 1 <en-us_topic_0000001811490985__ta1ceaf79532d439ead5e098dee6f7b7e>` explains the message security levels used in GaussDB(DWS). If logging output is sent to **syslog** or **eventlog**, severity is translated in GaussDB(DWS) as shown in the table.

.. _en-us_topic_0000001811490985__ta1ceaf79532d439ead5e098dee6f7b7e:

.. table:: **Table 1** Message Severity Levels

   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | Severity   | Description                                                                                                                                              | syslog  | eventlog    |
   +============+==========================================================================================================================================================+=========+=============+
   | debug[1-5] | Provides detailed debug information.                                                                                                                     | DEBUG   | INFORMATION |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | log        | Reports information of interest to administrators, for example, checkpoint activity.                                                                     | INFO    | INFORMATION |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | info       | Provides information implicitly requested by the user, for example, output from **VACUUM VERBOSE**.                                                      | INFO    | INFORMATION |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | notice     | Provides information that might be helpful to users, for example, notice of truncation of long identifiers and index created as part of the primary key. | NOTICE  | INFORMATION |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | warning    | Provides warnings of likely problems, for example, **COMMIT** outside a transaction block.                                                               | NOTICE  | WARNING     |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | error      | Reports an error that causes a command to terminate.                                                                                                     | WARNING | ERROR       |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | fatal      | Reports the reason that causes a session to terminate.                                                                                                   | ERR     | ERROR       |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+
   | panic      | Reports an error that caused all database sessions to terminate.                                                                                         | CRIT    | ERROR       |
   +------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+---------+-------------+

plog_merge_age
--------------

**Parameter description**: Specifies the output interval of performance log data.

**Type**: SUSET

.. important::

   This parameter value is in milliseconds. You are advised to set this parameter to a value that is a multiple of 1000. That is, the value is in seconds. Name extension of the performance log files controlled by this parameter is .prf. These log files are stored in the **$GAUSSLOG/gs_profile/**\ <*node_name*> directory. *node_name* is the value of **pgxc_node_name** in the **postgres.conf** file. You are advised not to use this parameter externally.

**Value range**: an integer ranging from 0 to INT_MAX. The unit is millisecond (ms).

-  **0** indicates that the current session will not output performance log data.
-  A value other than 0 indicates the output interval of performance log data. As the value decreases, more log data is generated, which negatively impacts performance.

**Default value**: **3s**

profile_logging_module
----------------------

**Parameter description**: Specifies the type of performance logs. When using this parameter, ensure that the value of **plog_merge_age** is not 0. This parameter is a session-level parameter, and you are not advised to use the **gs_guc** tool to set it. Only clusters of 8.1.3 and later versions support this function.

**Type**: USERSET

**Value range**: a string

**Default value**: OBS, HADOOP and REMOTE_DATANODE are enabled. MD is disabled. You can run the **SHOW profile_logging_module** command to view the value.

**Setting method**: First, you can run **SHOW profile_logging_module** to view which module is controllable. For example, the query output result is as follows:

::

   show profile_logging_module;
   profile_logging_module
   --------------------------------------------
   ALL,on(OBS,HADOOP,REMOTE_DATANODE),off(MD)(1 row)

Open the MD performance log and view the setting. The ALL identifier is equivalent to a shortcut operation. That is, logs of all modules can be enabled or disabled.

::

   set profile_logging_module='on(md)';
   SET

   show profile_logging_module;
   profile_logging_module
   ---------------------------------------------
   ALL,on(MD,OBS,HADOOP,REMOTE_DATANODE),off()(1 row)
