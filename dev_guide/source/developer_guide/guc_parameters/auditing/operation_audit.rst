:original_name: dws_04_0940.html

.. _dws_04_0940:

Operation Audit
===============

.. _en-us_topic_0000001098654960__section19376165411211:

audit_operation_exec
--------------------

**Parameter description**: Specifies whether to audit successful operations in GaussDB(DWS). Set this parameter as required.

**Type**: SIGHUP

**Value range**: a string

-  **none**: indicates that no audit item is configured. If any audit item is configured, **none** becomes invalid.
-  **all**: indicates that all successful operations are audited. This value overwrites the concurrent configuration of any other audit items. Note that even if this parameter is set to **all**, not all DDL operations are audited. You need to control the object level of DDL operations by referring to :ref:`audit_system_object <en-us_topic_0000001098654960__s3d7ad19c2a3647669dfa7f39540883bc>`.
-  **login**: indicates that successful logins are audited.
-  **logout**: indicates that user logouts are audited.
-  **database_process**: indicates that database startup, stop, switchover, and recovery operations are audited.
-  **user_lock**: indicates that successful locking and unlocking operations are audited.
-  **grant_revoke**: indicates that successful granting and reclaiming of a user's permission are audited.
-  **ddl**: indicates that successful DDL operations are audited. DDL operations are controlled at a fine granularity based on operation objects. Therefore, **audit_system_object** is used to control the objects whose DDL operations are to be audited. (The audit function takes effect as long as **audit_system_object** is configured, no matter whether **ddl** is set.)
-  **select**: indicates that successful SELECT operations are audited.
-  **copy**: indicates that successful COPY operations are audited.
-  **userfunc**: indicates that successful operations for user-defined functions, stored procedures, and anonymous blocks are audited.
-  **set**: indicates that successful SET operations are audited.
-  **transaction**: indicates that successful transaction operations are audited.
-  **vacuum**: indicates that successful VACUUM operations are audited.
-  **analyze**: indicates that successful ANALYZE operations are audited.
-  **explain**: indicates that successful EXPLAIN operations are audited.
-  **specialfunc**: indicates that successful calls to special functions are audited. Special functions include **pg_terminate_backend** and **pg_cancel_backend**.
-  **insert**: indicates that successful INSERT operations are audited.
-  **update**: indicates that successful UPDATE operations are audited.
-  **delete**: indicates that successful DELETE operations are audited.
-  **merge**: indicates that successful MERGE operations are audited.
-  **show**: indicates that successful SHOW operations are audited.
-  **checkpoint**: indicates that successful CHECKPOINT operations are audited.
-  **barrier**: indicates that successful BARRIER operations are audited.
-  **cluster**: indicates that successful CLUSTER operations are audited.
-  **comment**: indicates that successful COMMENT operations are audited.
-  **cleanconn**: indicates that successful CLEANCONNECTION operations are audited.
-  **prepare**: indicates that successful PREPARE, EXECUTE, and DEALLOCATE operations are audited.
-  **constraints**: indicates that successful CONSTRAINTS operations are audited.
-  **cursor**: indicates that successful cursor operations are audited.

**Default value**: **login**, **logout**, **database_process**, **user_lock**, **grant_revoke**, **set**, **transaction**, and **cursor**

.. important::

   -  You are advised to reserve **transaction**. Otherwise, statements in a transaction will not be audited.
   -  You are advised to reserve **cursor**. Otherwise, the **SELECT** statements in a cursor will not be audited. To audit the **SELECT** statement within transactions and cursors, retain both transaction and cursor audit items.
   -  The Data Studio client automatically encapsulates **SELECT** statements using **CURSOR**.

audit_operation_error
---------------------

**Parameter description**: Specifies whether to audit failed operations in GaussDB(DWS). Set this parameter as required.

**Type**: SIGHUP

**Value range**: a string

-  **none**: indicates that no audit item is configured. If any audit item is configured, **none** becomes invalid.
-  **syn_success**: synchronizes the :ref:`audit_operation_exec <en-us_topic_0000001098654960__section19376165411211>` configuration. To be specific, if the audit of a successful operation is configured, the corresponding failed operation is also audited. Note that even after **syn_success** is configured, you can continue to configure the audit of other failed operations. If **audit_operation_exec** is set to **all**, all failed operations are audited. If **audit_operation_exec** is set to **none**, **syn_success** is equivalent to **none**, that is, no audit item is configured.
-  **parse**: indicates that the failed command parsing is audited, including the timeout of waiting for a command execution.
-  **login**: indicates that failed logins are audited.
-  **user_lock**: indicates that failed locking and unlocking operations are audited.
-  **violation**: indicates that a user's access violation operations are audited.
-  **grant_revoke**: indicates that failed granting and reclaiming of a user's permission are audited.
-  **ddl**: indicates that failed DDL operations are audited. DDL operations are controlled at a fine granularity based on operation objects and configuration of :ref:`audit_system_object <en-us_topic_0000001098654960__s3d7ad19c2a3647669dfa7f39540883bc>`. Therefore, failed DDL operations of the type specified in :ref:`audit_system_object <en-us_topic_0000001098654960__s3d7ad19c2a3647669dfa7f39540883bc>` will be audited after **ddl** is configured.
-  **select**: indicates that failed SELECT operations are audited.
-  **copy**: indicates that failed COPY operations are audited.
-  **userfunc**: indicates that failed operations for user-defined functions, stored procedures, and anonymous blocks are audited.
-  **set**: indicates that failed SET operations are audited.
-  **transaction**: indicates that failed transaction operations are audited.
-  **vacuum**: indicates that failed VACUUM operations are audited.
-  **analyze**: indicates that failed ANALYZE operations are audited.
-  **explain**: indicates that failed EXPLAIN operations are audited.
-  **specialfunc**: indicates that failed calls to special functions are audited. Special functions include **pg_terminate_backend** and **pg_cancel_backend**.
-  **insert**: indicates that failed INSERT operations are audited.
-  **update**: indicates that failed UPDATE operations are audited.
-  **delete**: indicates that failed DELETE operations are audited.
-  **merge**: indicates that failed MERGE operations are audited.
-  **show**: indicates that failed SHOW operations are audited.
-  **checkpoint**: indicates that failed CHECKPOINT operations are audited.
-  **barrier**: indicates that failed BARRIER operations are audited.
-  **cluster**: indicates that failed CLUSTER operations are audited.
-  **comment**: indicates that failed COMMENT operations are audited.
-  **cleanconn**: indicates that failed CLEANCONNECTION operations are audited.
-  **prepare**: indicates that failed PREPARE, EXECUTE, and DEALLOCATE operations are audited.
-  **constraints**: indicates that failed CONSTRAINTS operations are audited.
-  **cursor**: indicates that failed cursor operations are audited.
-  **blacklist**: indicates that the blacklist execution failure is audited.

**Default value**: **login**

audit_inner_tool
----------------

**Parameter description**: Specifies whether to audit the operations of the internal maintenance tool in GaussDB(DWS).

**Type**: SIGHUP

**Value range**: Boolean

-  **on**: indicates that all operations of the internal maintenance tool are audited.
-  **off**: indicates that all operations of the internal maintenance tool are not audited.

**Default value**: **off**

.. _en-us_topic_0000001098654960__s3d7ad19c2a3647669dfa7f39540883bc:

audit_system_object
-------------------

**Parameter description**: Specifies whether to audit the CREATE, DROP, and ALTER operations on the GaussDB(DWS) database object. The GaussDB(DWS) database objects include databases, users, schemas, and tables. The operations on the database object can be audited by changing the value of this parameter.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 4194303

-  **0** indicates that the function of auditing the CREATE, DROP, and ALTER operations on the GaussDB(DWS) database object can be disabled.
-  Other values indicate that the CREATE, DROP, and ALTER operations on a certain or some GaussDB(DWS) database objects are audited.

**Value description**:

The value of this parameter is calculated by 22 binary bits. The 22 binary bits represent 22 types of GaussDB(DWS) database objects. If the corresponding binary bit is set to **0**, the CREATE, DROP, and ALTER operations on corresponding database objects are not audited. If it is set to **1**, the CREATE, DROP, and ALTER operations are audited. For details about the audit content represented by these 22 binary bits, see :ref:`Table 1 <en-us_topic_0000001098654960__t1597f30ea7e346dd979eb1cc3213b343>`.

**Default value**: **12303**

.. _en-us_topic_0000001098654960__t1597f30ea7e346dd979eb1cc3213b343:

.. table:: **Table 1** Meaning of each value for the **audit_system_object** parameter

   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Binary Bit            | Meaning                                                                                                          | Value Description                                                                                          |
   +=======================+==================================================================================================================+============================================================================================================+
   | Bit 0                 | Whether to audit the CREATE, DROP, and ALTER operations on databases.                                            | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 1                 | Whether to audit the CREATE, DROP, and ALTER operations on schemas.                                              | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 2                 | Whether to audit the CREATE, DROP, and ALTER operations on users.                                                | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 3                 | Whether to audit the CREATE, DROP, ALTER, and TRUNCATE operations on tables.                                     | -  **0** indicates that the CREATE, DROP, ALTER, and TRUNCATE operations on these objects are not audited. |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, ALTER, and TRUNCATE operations on these objects are audited.     |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 4                 | Whether to audit the CREATE, DROP, and ALTER operations on indexes.                                              | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 5                 | Whether to audit the CREATE, DROP, and ALTER operations on views.                                                | -  **0** indicates that the CREATE and DROP operations on these objects are not audited.                   |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE and DROP operations on these objects are audited.                       |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 6                 | Whether to audit the CREATE, DROP, and ALTER operations on triggers.                                             | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 7                 | Whether to audit the CREATE, DROP, and ALTER operations on procedures/functions.                                 | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 8                 | Whether to audit the CREATE, DROP, and ALTER operations on tablespaces.                                          | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 9                 | Whether to audit the CREATE, DROP, and ALTER operations on resource pools.                                       | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 10                | Whether to audit the CREATE, DROP, and ALTER operations on workloads.                                            | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 11                | Whether to audit the CREATE, DROP, and ALTER operations on SERVER FOR HADOOP objects.                            | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 12                | Whether to audit the CREATE, DROP, and ALTER operations on data sources.                                         | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  |                                                                                                            |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 13                | Whether to audit the CREATE, DROP, and ALTER operations on Node Groups.                                          | -  **0** indicates that the CREATE and DROP operations on these objects are not audited.                   |
   |                       |                                                                                                                  |                                                                                                            |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE and DROP operations on these objects are audited.                       |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 14                | Whether to audit the CREATE, DROP, and ALTER operations on ROW LEVEL SECURITY objects.                           | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 15                | Whether to audit the CREATE, DROP, and ALTER operations on types.                                                | -  **0** indicates that the CREATE, DROP, and ALTER operations on types are not audited.                   |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on types are audited.                       |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 16                | Whether to audit the CREATE, DROP, and ALTER operations on text search objects (configurations and dictionaries) | -  **0** indicates that the CREATE, DROP, and ALTER operations on text search objects are not audited.     |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on text search objects are audited.         |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 17                | Whether to audit the CREATE, DROP, and ALTER operations on directories.                                          | -  **0** indicates that the CREATE, DROP, and ALTER operations on directories are not audited.             |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on directories are audited.                 |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 18                | Whether to audit the CREATE, DROP, and ALTER operations on workloads.                                            | -  **0** indicates that the CREATE, DROP, and ALTER operations on types are not audited.                   |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on types are audited.                       |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 19                | Whether to audit the CREATE, DROP, and ALTER operations on redaction policies.                                   | -  **0** indicates that the CREATE, DROP, and ALTER operations on redaction policies are not audited.      |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on redaction policies are audited.          |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 20                | Whether to audit the CREATE, DROP, and ALTER operations on sequences.                                            | -  **0** indicates that the CREATE, DROP, and ALTER operations on sequences are not audited.               |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on sequences are audited.                   |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+
   | Bit 21                | Whether to audit the CREATE, DROP, and ALTER operations on nodes.                                                | -  **0** indicates that the CREATE, DROP, and ALTER operations on nodes are not audited.                   |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on nodes are audited.                       |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------+

enableSeparationOfDuty
----------------------

**Parameter description**: Specifies whether the separation of permissions is enabled.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates that the separation of permissions is enabled.
-  **off** indicates that the separation of permissions is disabled.

**Default value**: **off**

enable_grant_option
-------------------

**Parameter description**: Specifies whether the **with grant option** function can be used in security mode.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the **with grant option** function can be used in security mode.
-  **off** indicates that the **with grant option** function cannot be used in security mode.

**Default value**: **off**

enable_copy_server_files
------------------------

**Parameter description**: Specifies whether to enable the permission to copy server files.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates that the permission to copy server files is enabled.
-  **off** indicates that the permission to copy server files is disabled.

**Default value**: **true**

.. important::

   **COPY FROM**/**TO** *file* requires system administrator permissions. However, if the separation of permissions is enabled, system administrator permissions are different from initial user permissions. In this case, you can use **enable_copy_server_file** to control the **COPY** permission of system administrators to prevent escalation of their permissions.
