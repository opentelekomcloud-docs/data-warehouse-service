:original_name: dws_04_0921.html

.. _dws_04_0921:

Performance Statistics
======================

During database operation, accessing locks, disk I/O operations, and handling invalid messages can all be performance bottlenecks for the database. GaussDB(DWS) provides performance statistics methods that can help easily locate performance issues.

**Generating Performance Statistics Logs**
------------------------------------------

**Parameter description**: For each query, the following four parameters control the performance statistics of corresponding modules recorded in the server log:

-  The **og_parser_stats** parameter controls the performance statistics of a parser recorded in the server log.
-  The **log_planner_stats** parameter controls the performance statistics of a query optimizer recorded in the server log.
-  The **log_executor_stats** parameter controls the performance statistics of an executor recorded in the server log.
-  The **log_statement_stats** parameter controls the performance statistics of the whole statement recorded in the server log.

All these parameters can only provide assistant analysis for administrators, which are similar to the getrusage() of the Linux OS.

**Type**: SUSET

.. important::

   -  **log_statement_stats** records the total statement statistics while other parameters only record statistics about each statement.
   -  The **log_statement_stats** parameter cannot be enabled together with other parameters recording statistics about each statement.

**Value range**: Boolean

-  **on** indicates the function of recording performance statistics is enabled.
-  **off** indicates the function of recording performance statistics is disabled.

**Default value**: **off**
