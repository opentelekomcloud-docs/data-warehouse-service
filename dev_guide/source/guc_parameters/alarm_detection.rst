:original_name: dws_04_0918.html

.. _dws_04_0918:

Alarm Detection
===============

During cluster running, error scenarios can be detected in a timely manner to inform users as soon as possible.

enable_alarm
------------

**Parameter description**: Enables the alarm detection thread to detect the fault scenarios that may occur in the database.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates the alarm detection thread can be enabled.
-  **off** indicates the alarm detection thread cannot be enabled.

**Default value**: **on**

connection_alarm_rate
---------------------

**Parameter description**: Specifies the ratio restriction that the maximum number of allowed parallel connections to the database. The maximum number of concurrent connections to the database is :ref:`max_connections <en-us_topic_0000001188482316__s2d671f584b5647a19255e7c6a3d116aa>` x **connection_alarm_rate**.

**Type**: SIGHUP

**Value range**: a floating point number ranging from 0.0 to 1.0

**Default value**: **0.9**

alarm_report_interval
---------------------

**Parameter description**: Specifies the interval at which an alarm is reported.

**Type**: SIGHUP

**Value range**: a non-negative integer. The unit is second.

**Default value**: **10**
