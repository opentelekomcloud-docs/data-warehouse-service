:original_name: dws_01_0803.html

.. _dws_01_0803:

Using the SQL Editor to Connect to a Cluster
============================================

Data sources are used for cluster login. Currently, the DWS cluster supports two login modes: custom (**username + password**) and IAM. Custom login is the default login mode. With IAM account login, you create an IAM user in the database and use a token to log in.

-  IAM account login is only supported by clusters of 8.1.3.331 (included) to 8.2.0 (excluded), 8.2.1.100, and later versions.
-  If you log in to a cluster as a custom user or a IAM user, you can create a user or grant permissions to an existing user on the cluster user management page. For details, see :ref:`Creating a DWS Database and User <dws_01_01113>`.

Constraints and Limitations
---------------------------

-  Custom login constraints:

   -  Select a cluster for the new data source, enter the username and password, and test the connection. After the connection is tested, you can open the cluster data connection.
   -  You are advised to select **Remember password** during login. If the database is not specified, the **GaussDB** database is used by default.
   -  Depending on tenants and users, connection permissions are separated. Connections are visible to different sub-users. Only the creator of a connection can view it.

-  IAM account login constraints:

   -  Only IAM users who have been assigned the **DWS Database Access** role can log in to DWS clusters. In this case, you need to contact a user with the **DWS Administrator** permissions to grant you the role on the current page.
   -  The IAM user does not have any permissions after logging in to the DWS cluster database. You need to assign permissions to the IAM user on the user management page.

-  Connection timeout limit:

   -  The connection timeout period is set in the background. If no operation is performed within 30 minutes, you need to log in again.
   -  The connection uniquely caches the **user login ID and database name** to guarantee that each user connects to each database with one connection and performs each operation on one connection.
   -  It is not recommended to run SQL commands on the same database from multiple windows. This can cause delays because the database establishes the same connection, and each command can only be executed after the previous one has finished.

Procedure
---------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Data** > **SQL Editor**.
#. There are two panels on the left: **Data Warehouse** and **Custom**. Data Warehouse can only be logged in by IAM users with the **DWS Database Access** role. If the conditions are met, you can connect to a cluster database to perform operations.
#. Switch to the **Custom** panel and click **Add Data Source**. (Alternatively, access the **Dedicated Clusters** page, locate the row that contains the target cluster, and click **Log In** in the **Operation** column.) Set the parameters listed in the following table.

   .. table:: **Table 1** Parameters for adding a data source

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                                                                                                                                             | Example               |
      +=======================+=========================================================================================================================================================================================================================================================================================================================================================================================================================+=======================+
      | Data Source           | Data source name                                                                                                                                                                                                                                                                                                                                                                                                        | dws-demo              |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Cluster               | Cluster to be connected.                                                                                                                                                                                                                                                                                                                                                                                                | ``-``                 |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                                         |                       |
      |                       | If SSL authentication is enabled for the cluster, select **SSL authentication**.                                                                                                                                                                                                                                                                                                                                        |                       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Database              | Database name. For a newly created DWS cluster, you can enter **gaussdb** (name of the default database of the cluster) or enter the name of another database.                                                                                                                                                                                                                                                          | gaussdb               |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Username              | Username                                                                                                                                                                                                                                                                                                                                                                                                                | dbadmin               |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password              | Password of the user. This password is only used to create data sources and obtain data source connections for using the WEB-SQL editor. If **Remember password** is selected, the default password is used for login when the data source is opened. If **Remember password** is not selected, the password needs to be entered again when the data source is opened after the page is refreshed or the login expires. | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Confirm the information and click **Test Connection**. After the connection test is successful, click **OK**.
