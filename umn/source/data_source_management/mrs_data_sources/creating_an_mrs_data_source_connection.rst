:original_name: dws_01_0059.html

.. _dws_01_0059:

Creating an MRS Data Source Connection
======================================

Scenario
--------

Before GaussDB(DWS) reads data from MRS HDFS, you need to create an MRS data source connection that functions as a channel of transporting data warehouse cluster data and MRS cluster data.

Impact on the System
--------------------

-  You can create only one MRS data source connection in the data warehouse cluster at a time.
-  When an MRS data source connection is being created, the system automatically adds inbound and outbound rules to security groups of the data warehouse cluster and MRS cluster. Nodes in the same subnet can be accessed.
-  For the MRS cluster with Kerberos authentication enabled, the system automatically adds a **Machine-Machine** user that belongs to user group **supergroup** to the MRS cluster.

Prerequisites
-------------

-  You have created a data warehouse cluster and recorded the VPC and subnet where the cluster resides.
-  An MRS cluster of the analysis type has been created.

Procedure
---------

#. Log in to the public cloud management console.

#. Choose **Service List** > **EI Enterprise Intelligence** > **MapReduce Service** to enter the MRS management console and create a cluster.

   Configure parameters as required. For details, see "Cluster Operation Guide > Custom Creation of a Cluster" in the *MapReduce Service User Guide*.

   -  The VPC of the MRS cluster must be the same as that of the data warehouse cluster.
   -  MRS cluster versions 1.9.2, 2.1.0, 3.0.2-LTS, and 3.1.2-LTS are recommended.

      .. note::

         -  For clusters of version 8.1.1.300 and later, MRS clusters support versions 1.6.\ ``*``, 1.7.\ ``*``, 1.8.\ ``*``, 1.9.\ ``*``, 2.0.\ ``*``, 3.0.\ ``*``, 3.1.\ ``*``, and later (``*`` indicates a number).
         -  For clusters earlier than version 8.1.1.300, MRS clusters support versions 1.6.\ ``*``, 1.7.\ ``*``, 1.8.\ ``*``, 1.9.\ ``*``, and 2.0.\ ``*`` (``*`` indicates a number).

   -  Select the Hadoop component.

   If you already have a qualified MRS cluster, skip this step.

#. Choose **Service List** > **EI Enterprise Intelligence** > **GaussDB(DWS)**.

#. On the GaussDB(DWS) console, choose **Cluster** > **Dedicated Clusters**.

#. In the cluster list, click the name of a cluster. The **Cluster Information** page is displayed.

#. In the navigation tree on the left, choose **Data Sources** > **MRS Data Sources**.

#. Click **Create MRS Cluster Connection** and configure parameters.

   .. table:: **Table 1** MRS common connection parameters

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                   |
      +===================================+===============================================================================================================================================================================================================================================================================================+
      | Data Source                       | GaussDB(DWS) database server name. It can contain 3 to 63 characters, including lowercase letters, numbers, and underscores (_), and must start with a lowercase letter.                                                                                                                      |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Configuration Mode                | The way in which the system obtains files. The options are as follows:                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                               |
      |                                   | -  **MRS Account**: Configure the username and password of the Manager of the MRS cluster. The system will log in to the Manager and automatically download configuration and verification files. For more information, see :ref:`Table 2 <en-us_topic_0000001951848305__table146145019347>`. |
      |                                   | -  **File upload**: Download the configuration file from the Manager of the MRS cluster and manually upload it. You can use this method for Kerberos authentication. For more information, see :ref:`Table 3 <en-us_topic_0000001951848305__table64029343414>`.                               |
      |                                   |                                                                                                                                                                                                                                                                                               |
      |                                   |    .. note::                                                                                                                                                                                                                                                                                  |
      |                                   |                                                                                                                                                                                                                                                                                               |
      |                                   |       If you select **File upload**, ensure that MRS can communicate with the GaussDB(DWS) cluster.                                                                                                                                                                                           |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Database                          | Database where the data source is located.                                                                                                                                                                                                                                                    |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Description                       | Description of the connection.                                                                                                                                                                                                                                                                |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001951848305__table146145019347:

   .. table:: **Table 2** Parameters of the MRS Account mode

      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                                |
      +===================================+============================================================================================================================================================================================================================================================================================================================================================================================================+
      | MRS Data Source                   | Select an MRS cluster that can be connected to GaussDB(DWS) from the drop-down list box. By default, the custom, hybrid, and analytical MRS clusters that are in the same VPC and subnet as the current GaussDB(DWS) cluster and available to the current user are displayed.                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                            |
      |                                   | After you select an MRS cluster, the system automatically displays whether Kerberos authentication is enabled for the selected cluster. Click **View MRS Cluster** to view its detailed information.                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                            |
      |                                   | If the **MRS Data Source** drop-down list is empty, click **Create MRS Cluster** to create an MRS cluster.                                                                                                                                                                                                                                                                                                 |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | MRS Account                       | Account used when a GaussDB(DWS) cluster connects to an MRS cluster.                                                                                                                                                                                                                                                                                                                                       |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Password                          | Password of the connection user. If you change the password, you need to create a connection again.                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                            |
      |                                   | .. important::                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                            |
      |                                   |    NOTICE:                                                                                                                                                                                                                                                                                                                                                                                                 |
      |                                   |    Ensure the account has been used for logging in to MRS Manager. If you use a new account, you will be asked to change your password when you first log in. In this case, the MRS data source will fail to be configured.                                                                                                                                                                                |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Use a Machine-Machine Account     | Creates a machine-machine account named dws in MRS and uses it for interaction with MRS. This account is in the **supergroup** group and has all permissions. If the switch is toggled off, the configured man-machine account will be used. Ensure this account has the permission to access data, or a message will be displayed during data source access, indicating the required file does not exist. |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001951848305__table64029343414:

   .. table:: **Table 3** Parameters of the File upload mode

      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                  |
      +===================================+==============================================================================================================================================================================+
      | Authentication Credential         | Keytab file of a user A credential file downloaded from Manager of the MRS cluster. File name format: **Username_Timestamp_keytab.tar**                                      |
      |                                   |                                                                                                                                                                              |
      |                                   | -  **For MRS 2.x or earlier**, choose **System** > **Manage User**. In the **Operation** column of a user, choose **More** > **Download authentication credential**.         |
      |                                   | -  **For MRS 3.x or later**, choose **System** > **Permission** > **User**. In the **Operation** column of a user, choose **More** > **Download Authentication Credential**. |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client Profile                    | Client configuration files of HDFS, Hive, and hosts. When downloading the client, set **Select Client Type** to **Configuration Files Only**.                                |
      |                                   |                                                                                                                                                                              |
      |                                   | -  **For MRS 2.x or earlier**, choose **Services** and click **Download Client**.                                                                                            |
      |                                   | -  **For MRS 3.x or later**, choose **Homepage**. Click the **More** icon and choose **Download Client**.                                                                    |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK** to save the connection.

   **Configuration Status** turns to **Creating**. You can view the connection that is successfully created in the MRS data source list and the connection status is **Available**.

   .. note::

      -  In the **Operation** column, you can click **Update Configurations** to update **MRS Cluster Status** and **Configuration Status**. During configuration update, you cannot create a connection. The system checks whether the security group rule is correct. If the rule is incorrect, the system rectifies the fault. For details, see :ref:`Updating the MRS Data Source Configuration <dws_01_0156>`.
      -  In the **Operation** column, you can click **Delete** to delete the unnecessary connection. When deleting a connection, you need to manually delete the security group rule.
      -  If the security group rules are not deleted, nodes in the data warehouse cluster can still communicate with nodes in the MRS cluster. If you have strict requirements on network security, manually delete the rules.
