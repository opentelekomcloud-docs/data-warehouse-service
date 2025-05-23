:original_name: dws_04_0631.html

.. _dws_04_0631:

PG_USER_STATUS
==============

**PG_USER_STATUS** records the states of users that access to the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** PG_USER_STATUS columns

   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                                     |
   +=======================+==========================+=================================================================================================================+
   | roloid                | OID                      | ID of the role                                                                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------+
   | failcount             | Integer                  | Specifies the number of failed attempts.                                                                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------+
   | locktime              | Timestamp with time zone | Time at which the role is locked                                                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------+
   | rolstatus             | Smallint                 | Role state                                                                                                      |
   |                       |                          |                                                                                                                 |
   |                       |                          | -  **0**: normal                                                                                                |
   |                       |                          | -  **1** indicates that the role is locked for some time because the failed login attempts exceed the threshold |
   |                       |                          | -  **2** indicates that the role is locked by the administrator.                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------+
   | permspace             | Bigint                   | Size of the permanent table storage space used by a role in the current instance.                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------+
   | tempspace             | Bigint                   | Size of the temporary table storage space used by a role in the current instance.                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------+
