:original_name: dws_01_0074.html

.. _dws_01_0074:

Enabling Separation of Duties for DWS Database Users
====================================================

Scenario
--------

By default, the administrator specified when you create a DWS cluster is the database system administrator. The administrator can create other users and view the audit logs of the database. That is, separation of permissions is disabled.

To ensure cluster data security, DWS supports separation of duties for clusters. Different types of users have different permissions.

For details about the default permissions mode and the separation of permissions mode, see "DWS Database Security Management > DWS User and Permissions Management > Separation of Duties in DWS" in the *Data Warehouse Service (DWS) Developer Guide*.

Impact on the System
--------------------

-  After you modified the security parameters and the modifications take effect, the cluster may be restarted, which makes the cluster unavailable temporarily.

Prerequisites
-------------

To modify the cluster's security configuration, ensure that the following conditions are met:

-  The cluster status is **Available**, **To be restarted**, or **Unbalanced**.
-  The target cluster should not be undergoing any node additions, specification changes, configurations, upgrades, redistribution operations, or restarts.

Procedure
---------

#. Log in to the DWS console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, click the name of a cluster. In the navigation pane, choose **Security Settings**.

   By default, **Configuration Status** is **Synchronized**, which indicates that the latest database result is displayed.

#. On the **Security Settings** page, configure separation of permissions.

   When separation of permissions is enabled, configure the username and password for **Security Administrator** and **Audit Administrator**. Then the system automatically creates these two users. You can use these two users to connect to the database and perform database-related operations. **By default, this function is disabled.**

   .. table:: **Table 1** Security parameters

      +------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter              | Description                                                                                                                                                                      | Example Value         |
      +========================+==================================================================================================================================================================================+=======================+
      | Security Administrator | The username must meet the following requirements:                                                                                                                               | security_admin        |
      |                        |                                                                                                                                                                                  |                       |
      |                        | -  Consists of lowercase letters, digits, or underscores.                                                                                                                        |                       |
      |                        | -  Starts with a lowercase letter or an underscore.                                                                                                                              |                       |
      |                        | -  Contains 6 to 64 characters.                                                                                                                                                  |                       |
      |                        | -  Cannot be a keyword of the DWS database. For details about the keywords of the DWS database, see "SQL Reference" > "Keyword" in the *Data Warehouse Service Developer Guide*. |                       |
      +------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password               | The password complexity requirements are as follows:                                                                                                                             | ``-``                 |
      |                        |                                                                                                                                                                                  |                       |
      |                        | -  Contain 12 to 32 characters.                                                                                                                                                  |                       |
      |                        | -  Cannot be the username or the username spelled backwards.                                                                                                                     |                       |
      |                        | -  Contains at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``)             |                       |
      |                        | -  Perform the weak password check on the passwords that you have created.                                                                                                       |                       |
      +------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Confirm Password       | Enter the password of the security administrator again.                                                                                                                          | ``-``                 |
      +------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Audit Administrator    | The username must meet the following requirements:                                                                                                                               | audit_admin           |
      |                        |                                                                                                                                                                                  |                       |
      |                        | -  Consists of lowercase letters, digits, or underscores.                                                                                                                        |                       |
      |                        | -  Starts with a lowercase letter or an underscore.                                                                                                                              |                       |
      |                        | -  Contains 6 to 64 characters.                                                                                                                                                  |                       |
      |                        | -  Cannot be a keyword of the DWS database. For details about the keywords of the DWS database, see "SQL Reference" > "Keyword" in the *Data Warehouse Service Developer Guide*. |                       |
      +------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password               | The password complexity requirements are as follows:                                                                                                                             | ``-``                 |
      |                        |                                                                                                                                                                                  |                       |
      |                        | -  Contain 12 to 32 characters.                                                                                                                                                  |                       |
      |                        | -  Cannot be the username or the username spelled backwards.                                                                                                                     |                       |
      |                        | -  Contains at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``)             |                       |
      |                        | -  Passes the weak password check.                                                                                                                                               |                       |
      +------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Confirm Password       | Enter the password of the audit administrator again.                                                                                                                             | ``-``                 |
      +------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **Apply**.

#. In the displayed **Save Configuration** dialog box, select or deselect **Restart the cluster** and click **Save**.

   -  If you select **Restart the cluster**, the system saves the settings on the **Security Settings** page and restarts the cluster immediately. After the cluster is restarted, the security settings take effect immediately.
   -  If you do not select **Restart the cluster**, the system only saves the settings on the **Security Settings** page. Later, you need to manually restart the cluster for the security settings to take effect.

   After the security settings are complete, **Configuration Status** can be one of the following on the **Security Settings** page:

   -  **Applying**: The system is saving the settings.
   -  **Synchronized**: The settings have been saved and taken effect.
   -  **Take effect after restart**: The settings have been saved but have not taken effect. Restart the cluster for the settings to take effect.
