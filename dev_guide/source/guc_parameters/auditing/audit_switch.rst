:original_name: dws_04_0938.html

.. _dws_04_0938:

Audit Switch
============

audit_enabled
-------------

**Parameter description**: Specifies whether to enable or disable the audit process. After the audit process is enabled, the auditing information written by the background process can be read from the pipe and written into audit files.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the auditing function is enabled.
-  **off** indicates that the auditing function is disabled.

**Default value**: **on**

audit_space_limit
-----------------

**Parameter description**: Specifies the total disk space occupied by audit files.

**Type**: SIGHUP

**Value range**: an integer ranging from **1024 KB** to **1024 GB**. The unit is KB.

**Default value**: **1GB**
