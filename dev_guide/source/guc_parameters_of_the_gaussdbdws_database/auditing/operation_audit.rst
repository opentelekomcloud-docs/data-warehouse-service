:original_name: dws_04_0940.html

.. _dws_04_0940:

Operation Audit
===============

security_enable_options
-----------------------

**Parameter description**: Specifies whether **grant_to_public**, **grant_with_grant_option**, and **foreign_table_options** can be used in security mode. (This parameter is supported only by clusters of version 8.2.0 or later.)

**Type**: SIGHUP

**Value range**: a string

-  **on** indicates that **grant to public** can be used in security mode.
-  **on** indicates that **with grant option** can be used in security mode.
-  **foreign_table_options** allows users to perform operations on foreign tables in security mode without explicitly granting the **useft** permission to users.

**Default value**: empty

.. note::

   -  In a newly installed cluster, this parameter is left blank by default, indicating that none of **grant_to_public**, **grant_with_grant_option**, and **foreign_table_options** can be used in security mode.
   -  In upgrade scenarios, the default value of this parameter is forward compatible. If the default values of **enable_grant_public** and **enable_grant_option** are **ON** before the upgrade, the default value of **security_enable_options** is **grant_to_public, grant_with_grant_option** after the upgrade.
