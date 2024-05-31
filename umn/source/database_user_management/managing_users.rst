:original_name: dws_01_01113.html

.. _dws_01_01113:

Managing Users
==============

GaussDB(DWS) allows you to manage database users on the console. You can create, delete, and update database users and manage their permissions on the console.

.. note::

   -  If the current version does not support this feature, contact technical support to upgrade the version.

   -  After a cluster is created, the users or roles created with it cannot be modified.
   -  Before using this function, ensure that the cluster is available.

Creating a User
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. On the **Users** tab, click **Create User**. The user creation page is displayed.

   |image1|

#. .. _en-us_topic_0000001658828846__en-us_topic_0000001662557549_li1180614455343:

   Complete the user information as required, confirm the information, and click **Next**.

   -  **Username**: A username must start with a letter and can contain letters, numbers, and underscores (_). The length cannot exceed 63 characters.
   -  **Password**: A password must start with a letter and can contain letters, numbers, and underscores (_). The length cannot exceed 63 characters.

      .. note::

         Must contain at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``)

   -  **Maximum Connections**: The maximum number of database connections that a user can set up. The value **-1** indicates that the number of connections is not limited.
   -  **Expires**: Set the expiration time of the user permission.
   -  **System Administrator**: Whether the user has the system administrator rights.
   -  **Create Database**: Whether the user has the permission to create databases.
   -  **Create Role**: Whether the user has the permission to create users and roles.
   -  **Inherited Permissions**: Whether the user inherits role permissions from its group. **This function is enabled by default. You are advised to retain this setting.**

#. Select the role to be granted to the user and click **Next**.

#. Configure permissions not included in the roles of the user.

   Click **Add** to add a permission configuration. Select the database object type and corresponding database object, and select the permission to complete assignment For details about permission definitions, see "DCL Syntax > GRANT" in *SQL Syntax Reference*.

   |image2|

#. After the authorization is complete, click **Create**.

Modifying a User
----------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **User Management**.

#. In the user list, select a user and click **Modify**. The page for modifying user details is displayed.

#. Modify the user information. For details, see :ref:`User Information <en-us_topic_0000001658828846__en-us_topic_0000001662557549_li1180614455343>`. After confirming that the information is correct, click **Next**.

   |image3|

#. Select the role to be granted to the user and click **Next**.

#. Add or remove permissions as required.

   |image4|

#. Confirm the permissions. Click lick **Save**.

Deleting a User
---------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **User Management**.
#. Select a user from the user list and click **Delete**. A confirmation dialog box is displayed.
#. Click **OK**.

   .. note::

      If a user has dependencies on database objects, such as tables, that have not been deleted, the user will fail to be deleted.

.. |image1| image:: /_static/images/en-us_image_0000001759579345.png
.. |image2| image:: /_static/images/en-us_image_0000001759419497.png
.. |image3| image:: /_static/images/en-us_image_0000001835402214.png
.. |image4| image:: /_static/images/en-us_image_0000001759579393.png
