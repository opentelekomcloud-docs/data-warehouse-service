:original_name: dws_01_01114.html

.. _dws_01_01114:

Managing Roles
==============

Creating a Role
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. Click the **Roles** tab and click **Create Role**. The role creation page is displayed.

#. Configure role information. The parameters are described as follows:

   .. _en-us_topic_0000001924569204__table8901647311:

   .. table:: **Table 1** Parameters for configuring role information

      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Parameter            | Description                                                                                                                                               | Example Value |
      +======================+===========================================================================================================================================================+===============+
      | Role Name            | The value must start with a letter and can contain a maximum of 63 characters, including letters, digits, and underscores (_).                            | dws-demo      |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Expires              | Expiration time of the role permissions.                                                                                                                  | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | System Administrator | Indicates whether the role has the system administrator rights.                                                                                           | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Create Database      | Specifies whether the role has the permission to create databases.                                                                                        | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Create Role          | Specifies whether the role has the permission to create users and roles.                                                                                  | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
      | Inherit Permissions  | Indicates whether the role inherits the permissions from its role group. **This function is enabled by default. You are advised to retain this setting.** | ``-``         |
      +----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+

#. If all the parameters are correctly set, click **Next**.

#. Configure the permissions of the role.

   Click **Add** to add a permission configuration. Select the database object type and the corresponding objects. Then, select permissions. For details about permission definitions, see "DCL Syntax" > "GRANT" in the *GaussDB(DWS) SQL Overview*.

   |image1|

#. After the authorization is complete, click **Create**. The role is created.

Modifying a Role
----------------

#. Log in to the GaussDB(DWS) console. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**.
#. In the role list, select a user and click **Modify**. The page for modifying role details is displayed.
#. Modify the role information. For the parameter description, see :ref:`Table 1 <en-us_topic_0000001924569204__table8901647311>`.
#. If all the parameters are correctly set, click **Next**.
#. Configure permissions. Select a permission as required and click **Modify** in the row where the permission is located to add or remove the permission.
#. Confirm the permissions. Click **Save**.

Deleting a Role
---------------

**Prerequisites**

To prevent any problems with deleting a role, check for dependencies such as database objects beforehand. If there are any dependencies, delete them first before proceeding with the role deletion.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a role from the role list and click **Delete**. A confirmation dialog box is displayed.
#. Click **OK** to delete the role.

.. |image1| image:: /_static/images/en-us_image_0000001951848669.png
