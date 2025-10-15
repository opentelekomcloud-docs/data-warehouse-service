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

**Value range**: an integer ranging from **1024** to **1073741824**. The unit is KB.

**Default value**: **1GB**

audit_object_name_format
------------------------

**Parameter description**: Specifies the format of the object name displayed in the **object_name** field of audit logs.

**Type**: USERSET

**Value range**: enumerated values

-  **single** indicates that the **object_name** field displays a single object name, which is the name of the target object.
-  **all** indicates that the **object_name** field displays multiple object names.

**Default value**: single

.. note::

   If the default value is set to **all**, multiple object names are displayed for SELECT, DELETE, UPDATE, INSERT, MERGE, CREATE TABLE AS, CREATE VIEW AS, DROP USER... CASCADE, DROP OWNED BY... CASCADE, DROP SCHEMA... CASSCADE, DROP TABLE... CASCADE, DROP FOREIGN TABLE... CASCADE, and DROP VIEW... CASCADE.

audit_object_details
--------------------

**Parameter description**: whether to record the **object_details** field in audit logs. This field indicates the table name, column name, and column type in the audit statement. This parameter is supported only by clusters of version 8.2.1.100 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the **object_details** field is recorded during the audit.
-  **off** indicates that the **object_details** field is not recorded during the audit.

**Default value**: **off**

.. note::

   -  If this parameter is set to **on**, the table name, column name, and column type in the statement will be audited, which may affect the performance. So, exercise caution when setting this parameter to **on**.
   -  If this parameter is set to **on**, the **object_details** field records the following statements: **SELECT**, **DELETE**, **UPDATE**, **INSERT**, **MERGE**, **CREATE TABLE AS SELECT**, **GRANT**, and **DECLARE CURSOR**. **GRANT** statements that fail to be executed are not recorded.
