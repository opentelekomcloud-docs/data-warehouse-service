:original_name: dws_01_01113.html

.. _dws_01_01113:

Creating a GaussDB(DWS) Database and User
=========================================

The default database **gaussdb** of GaussDB(DWS) is not used as the customer's service database. You can use multiple databases to ensure service isolation. When you first connect to **gaussdb** as the system administrator (**dbadmin**), it is important to plan the service databases, users, and roles based on the service requirements. This involves creating a service and transferring any existing upstream service data to GaussDB(DWS).

A role is a set of permissions. For details about the relationship between users and roles, see Permissions Management in the *Developer Guide*. You can create common roles, such as a role for database creation, before creating a user. Then, you can assign the created role to the user.

Users, roles, and permissions can be exported. For details, see :ref:`Exporting a User <en-us_topic_0000002167905964__section1574533217170>`, :ref:`Exporting User Permissions <en-us_topic_0000002167905964__section9745632171714>`, :ref:`Exporting Roles <en-us_topic_0000002167905964__section112045503172>`, and :ref:`Exporting Role Permissions <en-us_topic_0000002167905964__section14204450191714>`.

Constraints and Limitations
---------------------------

-  Avoid having all business operations run under a single database user. Instead, plan different database users according to the business modules.
-  For better access control of different business modules, it is better to use multiple users and permissions instead of depending on the system administrator user to run business operations.
-  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Creating a Database
-------------------

You can use the DDL syntax or SQL editor to create a table.

-  DDL syntax: For details about the syntax, see "CREATE DATABASE".

Creating a Role
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.

#. In the navigation pane, choose **User Management**.

#. Click the **Roles** tab and click **Create Role**. The role creation page is displayed.

#. Configure role information. The parameters are described as follows:

   .. _en-us_topic_0000002167905964__table8901647311:

   .. table:: **Table 1** Parameters for configuring role information

      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Parameter            | Description                                                                                                                             | Example Value |
      +======================+=========================================================================================================================================+===============+
      | Role Name            | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).          | DWS-demo      |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Expires              | Expiration time of the role permissions.                                                                                                | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | System Administrator | Whether the role has the system administrator rights.                                                                                   | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Create Database      | Whether the role has the permission to create databases.                                                                                | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Create Role          | Whether the role has the permission to create users and roles.                                                                          | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Inherit Permissions  | Whether the role inherits the permissions from its role group. By default, this function is enabled and it is best to keep it that way. | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+

#. Confirm the settings and click **Next**.

#. Configure the permissions of the role.

   Click **Add** to add a permission configuration. Select the database object type and the corresponding objects. Then, select permissions. For details about permission definitions, see "DCL Syntax" > "GRANT" in *Data Warehouse Service (DWS) SQL Syntax Reference*.

#. After the authorization is complete, click **Create**.

Creating a Database User
------------------------

You can use the DDL syntax or create a table on the GaussDB(DWS) console. For details about the DDL syntax, see "CREATE USER".

.. note::

   -  If the current console does not support this feature, contact technical support.
   -  After a cluster is created, the users or roles created with it cannot be modified.
   -  Before using this function, ensure that the cluster is available.

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.

#. In the navigation pane, choose **User Management**.

#. On the **Users** page, click **Create User**.

#. Set the parameters on the **Configure Basic Settings** page.

   .. _en-us_topic_0000002167905964__table1157333942717:

   .. table:: **Table 2** Parameters on the Configure Basic Settings page

      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                | Example Value         |
      +=======================+============================================================================================================================================================================================+=======================+
      | Username              | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).                                                             | DWS-demo              |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password              | Enter a value that is 12 to 32 characters long and can contain letters, digits, underscores (_), and special characters.                                                                   | ``-``                 |
      |                       |                                                                                                                                                                                            |                       |
      |                       | .. note::                                                                                                                                                                                  |                       |
      |                       |                                                                                                                                                                                            |                       |
      |                       |    Your password must contain a minimum of three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``) |                       |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Maximum Connections   | Maximum number of connections between the user and the database. The value **-1** indicates that the number of connections is not limited.                                                 | -1                    |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Expires               | Expiration time of the user's permissions.                                                                                                                                                 | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | System Administrator  | Whether the user is a system administrator.                                                                                                                                                | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Database       | Whether the user has the permission to create databases.                                                                                                                                   | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Role           | Whether the user has the permission to create users and roles.                                                                                                                             | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Inherit Permissions   | Whether the user inherits permissions from its user group. By default, this function is enabled and it is best to keep it that way.                                                        | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Confirm the settings and click **Next**.

#. On the **Configure Roles** page, select the role to be assigned to the user and click **Next**.

#. Configure permissions not included in the roles of the user.

   Click **Add** to add a permission configuration. Select the database object type and corresponding database object, and select the permission to complete assignment. For details about permission definitions, see "DCL Syntax" > "GRANT" in *Data Warehouse Service (DWS) SQL Syntax Reference*.

#. After the authorization is complete, click **Create**.

Modifying a User
----------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. In the user list, select a user and click **Modify**. The page for modifying user details is displayed.
#. Modify the user information. For details, see :ref:`Table 2 <en-us_topic_0000002167905964__table1157333942717>`. After confirming that the information is correct, click **Next**.
#. Select the role to be granted to the user and click **Next**.
#. After selecting a permission type, you can click **Edit** in the **Operation** column and click **Modify** in the **Permission** column to add or remove a permission.
#. Confirm the permissions. Click **Save**.

Deleting a User
---------------

**Prerequisites**

To prevent any problems with deleting a user, check for dependencies between database objects (such as tables) beforehand. If there are any dependencies, delete them first before proceeding with the user deletion.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a user from the user list and click **Delete**. A confirmation dialog box is displayed. If you select **Forcibly delete and remove dependencies**, tables and other database objects under the current user will be transferred to the administrator account.
#. Click **OK**.

.. _en-us_topic_0000002167905964__section1574533217170:

Exporting a User
----------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Click **Export** in the upper part of the user list and select the number of records to be exported to export the user list.
#. Confirm the configurations and click **Export**.

.. _en-us_topic_0000002167905964__section9745632171714:

Exporting User Permissions
--------------------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a user from the user list and click **Export Permissions** to export the user permission list.

Modifying a Role
----------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. In the role list, select a user and click **Modify**. The page for modifying role details is displayed.
#. Modify the role information. For the parameter description, see :ref:`Table 1 <en-us_topic_0000002167905964__table8901647311>`.
#. Confirm the settings and click **Next**.
#. Configure permissions. Select a permission type as required, click **Edit** in the **Operation** column, and click **Modify** in the **Permission** column to add or remove permissions.
#. Confirm the permissions. Click **Save**.

Deleting a Role
---------------

**Prerequisites**

To prevent any problems with deleting a role, check for dependencies such as database objects beforehand. If there are any dependencies, delete them first before proceeding with the role deletion.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a role from the role list and click **Delete**. A confirmation dialog box is displayed.
#. Click **OK** to delete the role.

.. _en-us_topic_0000002167905964__section112045503172:

Exporting Roles
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management** and click **Roles** to switch to the role list page.
#. Click **Export** in the upper part of the role list and select the number of roles to be exported.
#. Confirm the information and click **Export**.

.. _en-us_topic_0000002167905964__section14204450191714:

Exporting Role Permissions
--------------------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management** and click **Roles** to switch to the role list page.
#. Select a user from the role list, click **Export Permissions**, and select the number of records to be exported.
