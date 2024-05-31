:original_name: dws_04_0940.html

.. _dws_04_0940:

Operation Audit
===============

audit_system_object
-------------------

**Parameter description**: Specifies whether to audit the CREATE, DROP, and ALTER operations on the GaussDB(DWS) database object. The GaussDB(DWS) database objects include databases, users, schemas, and tables. The operations on the database object can be audited by changing the value of this parameter.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 4194303

-  **0** indicates that the function of auditing the CREATE, DROP, and ALTER operations on the GaussDB(DWS) database object can be disabled.
-  Other values indicate that the CREATE, DROP, and ALTER operations on a certain or some GaussDB(DWS) database objects are audited.

**Value description**:

The value of this parameter is calculated by 22 binary bits. The 22 binary bits represent 22 types of GaussDB(DWS) database objects. If the corresponding binary bit is set to **0**, the CREATE, DROP, and ALTER operations on corresponding database objects are not audited. If it is set to **1**, the CREATE, DROP, and ALTER operations are audited. For details about the audit content represented by these 22 binary bits, see :ref:`Table 1 <en-us_topic_0000001188323698__t1597f30ea7e346dd979eb1cc3213b343>`.

**Default value**: **12303**

.. _en-us_topic_0000001188323698__t1597f30ea7e346dd979eb1cc3213b343:

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
   | Bit 5                 | Whether to audit the CREATE, DROP, and ALTER operations on views.                                                | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
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
   | Bit 13                | Whether to audit the CREATE, DROP, and ALTER operations on Node Groups.                                          | -  **0** indicates that the CREATE, DROP, and ALTER operations on these objects are not audited.           |
   |                       |                                                                                                                  |                                                                                                            |
   |                       |                                                                                                                  | -  **1** indicates that the CREATE, DROP, and ALTER operations on these objects are audited.               |
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

enable_grant_public
-------------------

**Parameter description**: Specifies whether to allow the **grant to public** function in security mode.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the **grant to public** function can be used in security mode.
-  **off** indicates that the **grant to public** function cannot be used in security mode.

**Default value**: **off**
