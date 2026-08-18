:original_name: dws_04_0921.html

.. _dws_04_0921:

Performance Statistics
======================

During database operations, issues such as lock contention and disk I/O overload can lead to performance bottlenecks. You can use the performance statistics parameters provided by DWS to quickly locate the performance issues. These parameters are used only to assist database administrators in rough analysis. They are similar to getrusage() in Linux.

.. _en-us_topic_0000001764491424__section199951114483:

log_parser_stats
----------------

**Parameter description**: Controls whether to record performance statistics about a parser in server logs.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the function of recording performance statistics is enabled.
-  **off** indicates the function of recording performance statistics is disabled.

**Default value**: **off**

.. _en-us_topic_0000001764491424__section18969171265014:

log_planner_stats
-----------------

**Parameter description**: Controls whether to record performance statistics about a query optimizer in server logs.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the function of recording performance statistics is enabled.
-  **off** indicates the function of recording performance statistics is disabled.

**Default value**: **off**

.. _en-us_topic_0000001764491424__section13322615115014:

log_executor_stats
------------------

**Parameter description**: Controls whether to record performance statistics about an executor in server logs.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the function of recording performance statistics is enabled.
-  **off** indicates the function of recording performance statistics is disabled.

**Default value**: **off**

log_statement_stats
-------------------

**Parameter description**: Controls whether to record performance statistics about an entire statement in server logs.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the function of recording performance statistics is enabled.
-  **off** indicates the function of recording performance statistics is disabled.

**Default value**: **off**

.. note::

   This parameter cannot be enabled together with :ref:`log_parser_stats <en-us_topic_0000001764491424__section199951114483>`, :ref:`log_planner_stats <en-us_topic_0000001764491424__section18969171265014>` or :ref:`log_executor_stats <en-us_topic_0000001764491424__section13322615115014>`.
