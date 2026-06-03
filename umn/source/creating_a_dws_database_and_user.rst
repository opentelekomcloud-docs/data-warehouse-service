:original_name: dws_01_01113.html

.. _dws_01_01113:

Creating a DWS Database and User
================================

The default database **gaussdb** of DWS is not used as the customer's service database. You can use multiple databases to ensure service isolation. When you first connect to **gaussdb** as the system administrator (**dbadmin**), it is important to plan the service databases, users, and roles based on the service requirements. This involves creating a service and transferring any existing upstream service data to DWS.

A role is a set of permissions. For details about the relationship between users and roles, see "DWS Database Permissions Management" in *Data Warehouse Service (DWS) Developer Guide*. You can create common roles, such as a role for database creation, before creating a user. Then, you can assign the created role to the user.

Users, roles, and permissions can be exported. For details, see :ref:`Exporting a User <en-us_topic_0000002235494476__section1574533217170>`, :ref:`Exporting User Permissions <en-us_topic_0000002235494476__section9745632171714>`, :ref:`Exporting Roles <en-us_topic_0000002235494476__section112045503172>`, and :ref:`Exporting Role Permissions <en-us_topic_0000002235494476__section14204450191714>`.

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

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.

#. In the navigation pane, choose **User Management**.

#. Click the **Roles** tab and click **Create Role**. The role creation page is displayed.

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

Creating a Database User
------------------------

You can use the DDL syntax or create a table on the DWS console. For details about the DDL syntax, see section "CREATE USER".

.. note::

   -  After a cluster is created, the users or roles created with it cannot be modified.
   -  Before using this function, ensure that the cluster is available.

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.

#. In the navigation pane, choose **User Management**.

#. On the **Users** page, click **Create User**.

#. Set the parameters on the **Configure Basic Settings** page.

   .. _en-us_topic_0000002235494476__table1157333942717:

   .. table:: **Table 2** Parameters on the Configure Basic Settings page

      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter                                            | Description                                                                                                                                                             | Example Value         |
      +======================================================+=========================================================================================================================================================================+=======================+
      | Username                                             | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).                                          | Dwsdemo               |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password                                             | Enter a value that is 12 to 32 characters long and can contain letters, digits, underscores (_), and special characters.                                                | ``-``                 |
      |                                                      |                                                                                                                                                                         |                       |
      |                                                      | .. note::                                                                                                                                                               |                       |
      |                                                      |                                                                                                                                                                         |                       |
      |                                                      |    -  Contains at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``) |                       |
      |                                                      |    -  Be different from the username or the username spelled backwards.                                                                                                 |                       |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Maximum Connections                                  | Maximum number of connections between the user and the database. The value **-1** indicates that the number of connections is not limited.                              | -1                    |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Expires                                              | Expiration time of the user's permissions.                                                                                                                              | ``-``                 |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Logical Clusters (A parameter for a logical cluster) | Select the logical cluster to which the user belongs from the drop-down list.                                                                                           | ``-``                 |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | System Administrator                                 | Whether the user is a system administrator.                                                                                                                             | ``-``                 |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Database                                      | Whether the user has the permission to create databases.                                                                                                                | ``-``                 |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Role                                          | Whether the user has the permission to create users and roles.                                                                                                          | ``-``                 |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Inherit Permissions                                  | Whether the user inherits permissions from its user group. By default, this function is enabled and it is best to keep it that way.                                     | ``-``                 |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | **Description**                                      | Enter the description of the user to be created. The description contains a maximum of 500 characters.                                                                  | ``-``                 |
      +------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Confirm the settings and click **Next**.

#. On the **Configure Roles** page, select the role to be assigned to the user and click **Next**.

#. Configure permissions not included in the roles of the user.

   Click **Add** to add a permission configuration. Select the database object type and corresponding database object, and select the permission to complete assignment. Confirm the information and click **Save**. For details about permission definitions, see "DCL Syntax" > "GRANT" in *Data Warehouse Service (DWS) SQL Syntax Reference*.

#. After the authorization is complete, click **Create**.

Modifying a User
----------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. In the user list, select a user and click **Modify**. The page for modifying user details is displayed.
#. Modify the user information. For details, see :ref:`Table 2 <en-us_topic_0000002235494476__table1157333942717>`. After confirming that the information is correct, click **Next**.
#. Select the role to be granted to the user and click **Next**.
#. After selecting a permission type, you can click **Edit** in the **Operation** column and click **Modify** in the **Permission** column to add or remove a permission.
#. Confirm the permissions. Click **Save**.

Deleting a User
---------------

To prevent any problems with deleting a user, check for dependencies between database objects (such as tables) beforehand. If there are any dependencies, delete them first before proceeding with the user deletion.

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a user from the user list and click **Delete**. A confirmation dialog box is displayed. If you select **Forcibly delete and remove dependencies**, tables, functions, and other database objects under the current user will be transferred to the administrator account.
#. Click **OK**.

.. _en-us_topic_0000002235494476__section1574533217170:

Exporting a User
----------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Click **Export** above the user list and set the number of records to be exported.
#. After confirming that the information is correct, click **Export** to export the user list.

.. _en-us_topic_0000002235494476__section9745632171714:

Exporting User Permissions
--------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Locate a user from the user list and click **Export Permissions**. In the displayed dialog box, enter the number of records to be exported.
#. After confirming that the information is correct, click **Export** to export the list of user permissions.

Modifying a Role
----------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. In the role list, select a user and click **Modify**. The page for modifying role details is displayed.
#. Modify the role information. For the parameter description, see :ref:`Table 1 <en-us_topic_0000002235494476__table8901647311>`.
#. Confirm the settings and click **Next**.
#. Configure permissions. Select a permission type as required, click **Edit** in the **Operation** column, and click **Modify** in the **Permission** column to add or remove permissions.
#. Confirm the permissions. Click **Save**.

Deleting a Role
---------------

To prevent any problems with deleting a role, check for dependencies such as database objects beforehand. If there are any dependencies, delete them first before proceeding with the role deletion.

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a role from the role list and click **Delete**. A confirmation dialog box is displayed.
#. Click **OK** to delete the role.

.. _en-us_topic_0000002235494476__section112045503172:

Exporting Roles
---------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management** and click the **Roles** tab.
#. Click **Export** above the role list and set the number of records to be exported.
#. After confirming that the information is correct, click **Export** to export the role list.

.. _en-us_topic_0000002235494476__section14204450191714:

Exporting Role Permissions
--------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The cluster information page is displayed.
#. In the navigation pane, choose **User Management** and click the **Roles** tab.
#. Locate a role from the role list and click **Export Permissions**. In the displayed dialog box, enter the number of records to be exported.
#. After confirming that the information is correct, click **Export** to export the role permissions.
