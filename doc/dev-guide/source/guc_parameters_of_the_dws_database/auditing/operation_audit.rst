:original_name: dws_04_0940.html

.. _dws_04_0940:

Operation Audit
===============

security_enable_options
-----------------------

**Parameter description:** This parameter lets you use the **grant_to_public**, **grant_with_grant_option**, and **foreign_table_options** functions. These functions are off by default for security. Set this parameter as needed. It works with clusters version 8.2.0 and later.

**Type**: SIGHUP

**Value range**: a string

-  **grant_to_public** indicates that **grant to public** can be used.
-  **grant_with_grant_option** indicates that **with grant option** can be used.
-  **foreign_table_options** allows users to perform operations on foreign tables without explicitly granting the **useft** permission to users.

**Default value**: empty

.. note::

   -  In a newly installed cluster, this parameter is left blank by default, indicating that none of **grant_to_public**, **grant_with_grant_option**, and **foreign_table_options** can be used.
   -  In upgrade scenarios, the default value of this parameter is forward compatible. If the default values of **enable_grant_public** and **enable_grant_option** are **ON** before the upgrade, the default value of **security_enable_options** is **grant_to_public, grant_with_grant_option** after the upgrade.
