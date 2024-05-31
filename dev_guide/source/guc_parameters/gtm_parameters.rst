:original_name: dws_04_0943.html

.. _dws_04_0943:

GTM Parameters
==============

log_min_messages
----------------

**Parameter description**: Specifies which level of messages will be written into server logs. Each level covers all the levels following it. The lower the level is, the fewer messages will be written into the log.

.. important::

   If the values of **client_min_messages** and **log_min_messages** are the same, they indicate different levels.

**Type**: SUSET

**Valid values**: enumerated values. Valid values are **debug**, **debug5**, **debug4**, **debug3**, **debug2**, **debug1**, **info**, **log**, **notice**, **warning**, **error**, **fatal**, and **panic**. For details about the parameters, see :ref:`Table 1 <en-us_topic_0000001233883395__ta1ceaf79532d439ead5e098dee6f7b7e>`.

**Default value**: **warning**

enable_alarm
------------

**Parameter description**: Specifies whether to enable the alarm detection thread to detect the fault scenarios that may occur in the database.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on**: Alarm detection thread is enabled.
-  **off**: Alarm detection thread is disabled.

**Default value**: **on**
