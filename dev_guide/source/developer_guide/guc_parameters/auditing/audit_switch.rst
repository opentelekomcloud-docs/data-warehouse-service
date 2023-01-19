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

audit_data_format
-----------------

**Parameter description**: Specifies the format of the audit log files. Currently, only the binary format is supported.

**Type**: POSTMASTER

**Value range**: a string

**Default value**: **binary**

audit_rotation_interval
-----------------------

**Parameter description**: Specifies the interval of creating an audit log file. If the difference between the current time and the time when the previous audit log file is created is greater than the value of **audit_rotation_interval**, a new audit log file will be generated.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to **INT_MAX/60**. The unit is min.

**Default value**: **1d**

.. important::

   Adjust this parameter only when required. Otherwise, **audit_resource_policy** may fail to take effect. To control the storage space and time of audit logs, set the :ref:`audit_resource_policy <en-us_topic_0000001098654728__section939915522551>`, :ref:`audit_space_limit <en-us_topic_0000001098654728__s199c0d0827ad412287098d139191a00a>`, and :ref:`audit_file_remain_time <en-us_topic_0000001098654728__section149961828185211>` parameters.

audit_rotation_size
-------------------

**Parameter description**: Specifies the maximum capacity of an audit log file. If the total number of messages in an audit log exceeds the value of **audit_rotation_size**, the server will generate a new audit log file.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 1024. The unit is MB.

**Default value**: **10 MB**

.. important::

   Adjust this parameter only when required. Otherwise, **audit_resource_policy** may fail to take effect. To control the storage space and time of audit logs, set the **audit_resource_policy**, **audit_space_limit**, and **audit_file_remain_time** parameters.

.. _en-us_topic_0000001098654728__section939915522551:

audit_resource_policy
---------------------

**Parameter description**: Specifies the policy for determining whether audit logs are preferentially stored by space or time.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that audit logs are preferentially stored by space. A maximum of :ref:`audit_space_limit <en-us_topic_0000001098654728__s199c0d0827ad412287098d139191a00a>` logs can be stored.
-  **off** indicates that audit logs are preferentially stored by time. A minimum duration of :ref:`audit_file_remain_time <en-us_topic_0000001098654728__section149961828185211>` logs must be stored. If the value of **audit_file_remain_time** is too large, the disk space occupied by stored audit logs reaches the value of **audit_space_limit**. In this case, the earliest audit files are deleted.

**Default value**: **on**

.. _en-us_topic_0000001098654728__section149961828185211:

audit_file_remain_time
----------------------

**Parameter description**: Specifies the minimum duration required for recording audit logs. This parameter is valid only when :ref:`audit_resource_policy <en-us_topic_0000001098654728__section939915522551>` is set to **off**.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 730. The unit is day. **0** indicates that the storage duration is not limited.

**Default value**: **90**

.. _en-us_topic_0000001098654728__s199c0d0827ad412287098d139191a00a:

audit_space_limit
-----------------

**Parameter description**: Specifies the total disk space occupied by audit files.

**Type**: SIGHUP

**Value range**: an integer ranging from **1024 KB** to **1024 GB**. The unit is KB.

**Default value**: **1GB**

audit_file_remain_threshold
---------------------------

**Parameter description**: Specifies the maximum number of audit files in the audit directory.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 1048576

**Default value**: **1048576**

.. important::

   Ensure that the value of this parameter is **1048576**. If the value is changed, the **audit_resource_policy** parameter may not take effect. To control the storage space and time of audit logs, use the **audit_resource_policy**, **audit_space_limit**, and **audit_file_remain_time** parameters.
