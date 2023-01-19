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

-  You have created a data warehouse cluster and recorded the AZ, VPC, and subnet where the cluster resides.
-  An MRS cluster of the analysis type has been created.

Procedure
---------

#. Log in to the public cloud management console.

#. Choose **Service List** > **EI Enterprise Intelligence** > **MapReduce Service** to enter the MRS management console and create a cluster.

   Configure parameters as required. For details, see "Cluster Operation Guide > Custom Creation of a Cluster" in the *MapReduce Service User Guide*.

   -  The AZ, VPC, and subnet of the MRS cluster must be the same as those of the data warehouse cluster.
   -  **Cluster Type** must be **Analysis Cluster**.
   -  **Cluster Version** supports **MRS 1.9.2** (recommended).

      .. note::

         **Cluster Version** can also be set to 1.6.\ *x*, 1.7.\ *x*, 1.8.\ *x*, or 2.0.\ *x*.

   -  In the **Analysis Components** area, select **Hive**, **Tez**, and **Spark2x**.

   .. note::

      If you enable Kerberos authentication for an MRS cluster, use MRS Manager to create a user for interconnecting GaussDB(DWS) with the system after the MRS cluster is created. The user type must be **Human-Machine** and the user, user group **hadoop**, and role **Manager_administrator** must be bound together. The user password must be changed on the MRS Manager page after the user is created.

   If you already have a qualified MRS cluster, skip this step.

#. Choose **Service List** > **EI Enterprise Intelligence** > **GaussDB(DWS)**.

#. On the GaussDB(DWS) management console, click **Clusters**.

#. In the cluster list, click the name of a cluster. On the page that is displayed, click the **MRS Data Sources** tab.

#. Click **Create MRS Cluster Connection** and configure parameters.


   .. figure:: /_static/images/en-us_image_0000001180320269.png
      :alt: **Figure 1** Creating an MRS data source

      **Figure 1** Creating an MRS data source

   .. table:: **Table 1** MRS cluster connection parameters

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                          |
      +===================================+======================================================================================================================================================================================================================================+
      | **MRS Data Source**               | Specifies the MRS cluster to which GaussDB(DWS) can connect. By default, all available analytic MRS clusters that are in the same VPC and subnet as the current data warehouse cluster and in the **Available** state are displayed. |
      |                                   |                                                                                                                                                                                                                                      |
      |                                   | After you select an MRS cluster, the system automatically displays whether Kerberos authentication is enabled for the selected cluster. Click **View MRS Cluster** to view its detailed information.                                 |
      |                                   |                                                                                                                                                                                                                                      |
      |                                   | If the **MRS Data Source** drop-down list is empty, click **Create MRS Cluster** to create an MRS cluster.                                                                                                                           |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | **MRS Account**                   | Specifies the account used when a data warehouse cluster connects to an MRS cluster. This parameter is available only when Kerberos authentication is selected for the MRS cluster.                                                  |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | **Password**                      | Specifies the password of the connection user. If you change the password, you need to create a connection again. This parameter is valid only for clusters with MRS Kerberos authentication enabled.                                |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | **Description**                   | Describes the connection.                                                                                                                                                                                                            |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK** to save the connection.

   **Configuration Status** turns to **Creating**. You can view the connection that is successfully created in the MRS data source list and the connection status is **Available**.

   .. note::

      -  In the **Operation** column, you can click **Update Configurations** to update **MRS Cluster Status** and **Configuration Status**. During configuration update, you cannot create a connection. The system checks whether the security group rule is correct. If the rule is incorrect, the system rectifies the fault. For details, see :ref:`Updating the MRS Data Source Configuration <dws_01_0156>`.
      -  In the **Operation** column, you can click **Delete** to delete the unnecessary connection. When deleting a connection, you need to manually delete the security group rule.
