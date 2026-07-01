:original_name: dws_01_01113.html

.. _dws_01_01113:

Creating a DWS Database and User
================================

The default database **gaussdb** of DWS is not used as the customer's service database. You can use multiple databases to ensure service isolation. When you first connect to **gaussdb** as the system administrator (**dbadmin**), it is important to plan the service databases, users, and roles based on the service requirements. This involves creating a service and transferring any existing upstream service data to DWS.

A role is a set of permissions. For details about the relationship between users and roles, see "DWS Database Permissions Management" in *Data Warehouse Service (DWS) Developer Guide*. You can create common roles, such as a role for database creation, before creating a user. Then, you can assign the created role to the user.

Users, roles, and permissions can be exported. For details, see :ref:`Managing Roles <en-us_topic_0000002235494476__section155643567515>` and :ref:`Managing Database Users <en-us_topic_0000002235494476__section1149516123502>`.

Constraints and Limitations
---------------------------

-  Avoid having all business operations run under a single database user. Instead, plan different database users according to the business modules.
-  For better access control of different business modules, it is better to use multiple users and permissions instead of depending on the system administrator user to run business operations.
-  For more information about development and design specifications, see "DWS Development Design Specifications" in the *Data Warehouse Service (DWS) Developer Guide*.

Creating a Database
-------------------

You can use the DDL syntax or SQL editor to create a table.

-  DDL syntax creation: see section "CREATE DATABASE".

Creating a Role
---------------

#. Log in to the DWS console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. Click the **Roles** tab. On this tab page, click **Create Role**.

#. Configure role information. The parameters are described as follows:

   .. _en-us_topic_0000002235494476__table8901647311:

   .. table:: **Table 1** Parameters for configuring role information

      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Parameter            | Description                                                                                                                             | Example Value |
      +======================+=========================================================================================================================================+===============+
      | Role Name            | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).          | dwsdemo       |
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
      | **Description**      | Enter the description of the role to be created. The description contains a maximum of 500 characters.                                  | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+---------------+

#. Confirm the settings and click **Next**.

#. Configure the permissions of the role.

   Click **Add** to add a permission configuration. Select the database object type and the corresponding objects. Then select permissions and click **Save**. For details about permission definitions, see "DCL Syntax" > "GRANT" in *Data Warehouse Service (DWS) SQL Syntax Reference*.

#. After the authorization is complete, click **Create**.

.. _en-us_topic_0000002235494476__section33942019436:

Creating a Database User
------------------------

Before using this function, ensure that the cluster is available. You can use the DDL syntax or create a table on the DWS console. For details about the DDL syntax, see section "CREATE USER".

#. Log in to the DWS console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. On the **Users** page, click **Create User**.

#. Set the parameters on the **Configure Basic Settings** page.

   .. _en-us_topic_0000002235494476__table1157333942717:

   .. table:: **Table 2** Parameters on the Configure Basic Settings page

      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter                                            | Description                                                                                                                                                          | Example Value         |
      +======================================================+======================================================================================================================================================================+=======================+
      | Username                                             | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).                                       | Dwsdemo               |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password                                             | Enter a value that is 12 to 32 characters long and can contain letters, digits, underscores (_), and special characters.                                             | ``-``                 |
      |                                                      |                                                                                                                                                                      |                       |
      |                                                      | -  Contains at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``) |                       |
      |                                                      | -  Be different from the username or the username spelled backwards.                                                                                                 |                       |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Maximum Connections                                  | Maximum number of connections between the user and the database. The value **-1** indicates that the number of connections is not limited.                           | -1                    |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Expires                                              | Expiration time of the user's permissions.                                                                                                                           | ``-``                 |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Logical Clusters (A parameter for a logical cluster) | Select the logical cluster to which the user belongs from the drop-down list.                                                                                        | ``-``                 |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | System Administrator                                 | Whether the user is a system administrator.                                                                                                                          | ``-``                 |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Database                                      | Whether the user has the permission to create databases.                                                                                                             | ``-``                 |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Role                                          | Whether the user has the permission to create users and roles.                                                                                                       | ``-``                 |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Inherit Permissions                                  | Whether the user inherits permissions from its user group. By default, this function is enabled and it is best to keep it that way.                                  | ``-``                 |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | **Description**                                      | Enter the description of the user to be created. The description contains a maximum of 500 characters.                                                               | ``-``                 |
      +------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Confirm the settings and click **Next**.

#. On the **Configure Roles** page, select the role to be assigned to the user and click **Next**.

#. Configure permissions not included in the roles of the user.

   Click **Add** to add a permission configuration. Select the database object type and corresponding database object, and select the permission to complete assignment. Confirm the information and click **Save**. For details about permission definitions, see "DCL Syntax" > "GRANT" in *Data Warehouse Service (DWS) SQL Syntax Reference*.

#. After the authorization is complete, click **Create**. After a cluster is created, the users or roles created with it cannot be modified.

.. _en-us_topic_0000002235494476__section155643567515:

Managing Roles
--------------

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**.
#. Click the **Roles** tab and perform operations on roles. The following tables lists the supported operations.

   .. table:: **Table 3** Role management operations

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operation                         | Description                                                                                                                                                                                              |
      +===================================+==========================================================================================================================================================================================================+
      | Modifying a role                  | In the role list, locate the role you want to modify and click **Modify** in the **Operation** column. Confirm the permissions. Click **Save**.                                                          |
      |                                   |                                                                                                                                                                                                          |
      |                                   | -  Modify information about the role. For details, see :ref:`Table 1 <en-us_topic_0000002235494476__table8901647311>`.                                                                                   |
      |                                   | -  Configure permissions. Select a permission type as required, click **Edit** in the **Operation** column, and click **Modify** in the **Permission** column to add or remove permissions.              |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Exporting roles                   | Click **Export** above the role list. In the displayed dialog box, set the parameters and click **Export**.                                                                                              |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Exporting role permissions        | Locate a role in the role list and click **Export Authority**. In the displayed dialog box, set **Export Data Volume** and click **Export** to export permissions of the role.                           |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Deleting a role                   | In the role list, locate the role you want to delete and click **Delete** in the **Operation** column. In the displayed dialog box, click **OK** to delete the role.                                     |
      |                                   |                                                                                                                                                                                                          |
      |                                   | To prevent any problems with deleting a role, **check for dependencies such as database objects** beforehand. If there are any dependencies, delete them first before proceeding with the role deletion. |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000002235494476__section1149516123502:

Managing Database Users
-----------------------

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**. Perform the following operations on database users.

   .. table:: **Table 4** User management operations

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operation                         | Description                                                                                                                                                                                                 |
      +===================================+=============================================================================================================================================================================================================+
      | Modifying a user                  | In the user list, locate a user and click **Modify**. Configure permissions and click **Save**.                                                                                                             |
      |                                   |                                                                                                                                                                                                             |
      |                                   | -  Modify information about the user. For details, see :ref:`Table 2 <en-us_topic_0000002235494476__table1157333942717>`.                                                                                   |
      |                                   | -  Configure the roles you want to assign to the user.                                                                                                                                                      |
      |                                   | -  After selecting a permission type, you can click **Edit** in the **Operation** column and click **Modify** in the **Permission** column to add or remove a permission.                                   |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Exporting users                   | Click **Export** above the user list. In the displayed dialog box, set **Export Data Volume** and click **Export**.                                                                                         |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Exporting user permissions        | Locate a user in the user list and click **Export Authority**. In the displayed dialog box, set **Export Data Volume** and click **Export** to export permissions of the user.                              |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Deleting a user                   | In the user list, locate a user, click **More** in the **Operation** column, and select **Delete**. In the displayed dialog box, click **OK**.                                                              |
      |                                   |                                                                                                                                                                                                             |
      |                                   | -  To prevent any problems with deleting a user, **check for dependencies such as database objects** beforehand. If there are any dependencies, delete them first before proceeding with the user deletion. |
      |                                   | -  If you select **Forcibly delete and remove dependencies**, tables, functions, and other database objects under the current user will be transferred to the administrator account.                        |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
