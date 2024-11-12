:original_name: dws_01_01113.html

.. _dws_01_01113:

Managing Users
==============

GaussDB(DWS) allows you to manage database users on the console. You can create, delete, and update database users and manage their permissions on the console.

Constraints and Limitations
---------------------------

-  If the current console does not support this feature, contact technical support.
-  After a cluster is created, the users or roles created with it cannot be modified.
-  Before using this function, ensure that the cluster is available.

Creating a User
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. On the **Users** tab, click **Create User**. The user creation page is displayed.

#. The Basic Settings page is displayed. The parameters are described as follows:

   .. _en-us_topic_0000001951848497__table1157333942717:

   .. table:: **Table 1** Basic parameters of user information

      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                           | Example Value         |
      +=======================+=======================================================================================================================================================================+=======================+
      | User Name             | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).                                        | Dws-demo              |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password              | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).                                        | ``-``                 |
      |                       |                                                                                                                                                                       |                       |
      |                       | .. note::                                                                                                                                                             |                       |
      |                       |                                                                                                                                                                       |                       |
      |                       |    Contains at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\\=-``) |                       |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Maximum Connections   | Maximum number of connections between the user and the database. The value **-1** indicates that the number of connections is not limited.                            | -1                    |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Expires               | Expiration time of the user's permissions.                                                                                                                            | ``-``                 |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | System Administrator  | Indicates whether the user is a system administrator.                                                                                                                 | ``-``                 |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Database       | Specifies whether the user has the permission to create databases.                                                                                                    | ``-``                 |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Create Role           | Specifies whether the user has the permission to create users and roles.                                                                                              | ``-``                 |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Inherit Permissions   | Specifies whether the user inherits permissions from its user group. **This function is enabled by default. You are advised to retain this setting.**                 | ``-``                 |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. If all the parameters are correctly set, click **Next**.

#. On the **Configure Roles** page, select the role to be assigned to the user and click **Next**.

#. Configure permissions not included in the roles of the user.

   Click **Add** to add a permission configuration. Select the database object type and corresponding database object, and select the permission to complete assignment For details about permission definitions, see "DCL Syntax" > "GRANT" in the *GaussDB(DWS) SQL Overview*.

#. After the authorization is complete, click **Create**.

Modifying a User
----------------

#. Log in to the GaussDB(DWS) console. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**.
#. In the user list, select a user and click **Modify**. The page for modifying user details is displayed.
#. Modify the user information. For details, see :ref:`Table 1 <en-us_topic_0000001951848497__table1157333942717>`. After confirming that the information is correct, click **Next**.
#. Select the role to be granted to the user and click **Next**.
#. After selecting a permission type, you can click **Modify** in the row where the permission is located to add or remove the permission.
#. Confirm the permissions. Click **Save**.

Deleting a User
---------------

**Prerequisites**

To prevent any problems with deleting a user, check for dependencies between database objects (such as tables) beforehand. If there are any dependencies, delete them first before proceeding with the user deletion.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a user from the user list and click **Delete**. A confirmation dialog box is displayed.
#. Click **OK**.
