:original_name: dws_01_01114.html

.. _dws_01_01114:

Managing Roles
==============

Creating a Role
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. Click the **Roles** tab and click **Create Role**. The role creation page is displayed.

   |image1|

#. Complete the role information, confirm the information, and click **Next**.

   -  **Role Name**: A username must start with a letter and can contain letters, numbers, and underscores (_). The length cannot exceed 63 characters.
   -  **Expires**: Set the expiration time of the role permission.
   -  **System Administrator**: Whether the role has the system administrator rights.
   -  **Create Database**: Whether the role has the permission to create databases.
   -  **Create Role**: Whether the role has the permission to create users and roles.
   -  **Inherit Permissions**: Whether a role inherits permissions from its group. This function is enabled by default. You are advised to retain this setting.

#. Configure the permissions of the role.

   Click **Add** to add a permission configuration. Select the database object type and the corresponding objects. Then, select permissions. For details about permission definitions, see "DCL Syntax > GRANT" in *SQL Syntax Reference*.

   |image2|

#. After the authorization is complete, click **Create**. The role is created.

Modifying a Role
----------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. In the role list, select a user and click **Modify**. The page for modifying role details is displayed.

#. Modify the role information, confirm the information, and click **Next**.

   -  **Role Name**: A username must start with a letter and can contain letters, numbers, and underscores (_). The length cannot exceed 63 characters.
   -  **Expires**: Set the expiration time of the role permission.
   -  **System Administrator**: Whether the role has the system administrator rights.
   -  **Create Database**: Whether the role has the permission to create databases.
   -  **Create Role**: Whether the role has the permission to create users and roles.
   -  **Inherit Permissions**: Whether a role inherits the permissions from its group. This function is enabled by default. You are advised to retain this setting.

#. Add or remove permissions as required.

   |image3|

#. Confirm the permissions. Click lick **Save**.

Deleting a Role
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a role from the role list and click **Delete**. A confirmation dialog box is displayed.
#. Click **OK** to delete the role.

   .. note::

      If the role has dependencies, such as database objects, that have not been deleted, the role will fail to be deleted.

.. |image1| image:: /_static/images/en-us_image_0000001759419545.png
.. |image2| image:: /_static/images/en-us_image_0000001711660448.png
.. |image3| image:: /_static/images/en-us_image_0000001711660452.png
