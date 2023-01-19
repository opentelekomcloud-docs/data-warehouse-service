:original_name: dws_04_0914.html

.. _dws_04_0914:

Logging Destination
===================

log_truncate_on_rotation
------------------------

**Parameter description**: Specifies the writing mode of the log files when **logging_collector** is set to **on**.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that GaussDB(DWS) overwrites the existing log file of the same name on the server.
-  **off** indicates that GaussDB(DWS) appends the log messages to the existing log file of the same name on the server.

**Default value**: **off**

.. note::

   Example:

   Assume that you plan to keep logs in a period of 7 days, one log file is generated per day, log files generated on Monday are named **server_log.Mon** and named **server_log.Tue** on Tuesday (others are named in the same way), and log files generated on the same day in different weeks are overwritten. Implement the plan by performing the following operations: set **log_filename** to **server_log.%a**, **log_truncate_on_rotation** to **on**, and **log_rotation_age** to **1440** (indicating the valid duration of the log file is 24 hours).

log_rotation_age
----------------

**Parameter description**: Specifies the interval for creating a log file when **logging_collector** is set to **on**. If the difference between the current time and the time when the previous audit log file is created is greater than the value of **log_rotation_age**, a new log file will be generated.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 24 days. The unit is min, h, or d. **0** indicates that the time-based creation of new log files is disabled.

**Default value**: **1d**

log_rotation_size
-----------------

**Parameter description**: Specifies the maximum size of a server log file when **logging_collector** is set to **on**. If the total size of messages in a server log exceeds the capacity of the server log file, a log file will be generated.

**Type**: SIGHUP

**Value range**: an integer ranging from INT_MAX to 1024. The unit is KB.

**0** indicates the capacity-based creation of new log files is disabled.

**Default value**: **20 MB**

event_source
------------

**Parameter description**: Specifies the identifier of the GaussDB(DWS) error messages in logs when **log_destination** is set to **eventlog**.

**Type**: POSTMASTER

**Value range**: a string

**Default value**: **PostgreSQL**
