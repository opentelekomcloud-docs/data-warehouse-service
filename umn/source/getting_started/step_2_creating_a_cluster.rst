:original_name: dws_01_0013.html

.. _dws_01_0013:

Step 2: Creating a Cluster
==========================

Before using GaussDB(DWS) to analyze data, create a cluster. A cluster consists of multiple nodes in the same subnet. These nodes jointly provide services. This section describes how to create a GaussDB(DWS) cluster.

Creating a Cluster
------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane, choose **Cluster** > **Dedicated Clusters**.

#. On the **Dedicated Clusters** page, click **Create Cluster** in the upper right corner.

#. Select the region to which the cluster to be created belongs.

   -  **Region**: Select the current working area of the cluster.
   -  **AZ**: Retain the default value.

#. Configure node parameters.

   -  **Resource**: For example, **Standard**.
   -  **CPU Architecture**: Select a CPU architecture based on your requirements, for example, **x86**.
   -  **Node Flavor**: Retain the default value.
   -  **Nodes**: Retain the default value. At least **3** nodes are required.

#. Configure cluster parameters.

   -  **Cluster Name**: Enter **dws-demo**.
   -  **Cluster Version**: The current cluster version is displayed and cannot be changed.
   -  **Default Database**: The value is **gaussdb**, which cannot be changed.
   -  **Administrator Account**: The default value is **dbadmin**. Use the default value. After a cluster is created, the client uses this database administrator account and its password to connect to the cluster's database.
   -  **Administrator Password**: Enter the password.
   -  **Confirm Password**: Enter the database administrator password again.
   -  **Database Port**: Use the default port number. This port is used by the client or application to connect to the cluster's database.


   .. figure:: /_static/images/en-us_image_0000001231389497.png
      :alt: **Figure 1** Configuring the cluster

      **Figure 1** Configuring the cluster

#. Configure network parameters.

   -  **VPC**: You can select an existing VPC from the drop-down list. If no VPC has been configured, click **View VPC** to enter the VPC management console to create one, for example, **vpc-dws**. Then, go back to the page for creating a cluster on the GaussDB(DWS) console, click |image1| next to the **VPC** drop-down list, and select the new VPC.

   -  **Subnet**: When you create a VPC, a subnet is created by default. You can select the corresponding subnet.

   -  **Security Group**: Select **Automatic creation**.

      The automatically created security group is named **GaussDB(DWS)**-<*Cluster name*>-<*GaussDB(DWS) cluster database port*>. The outbound allows all access requests, while the inbound enables only **Database Port** for access requests from clients or applications.

      If you select a custom security group, add an inbound rule to it to enable **Database Port** for client hosts to access GaussDB(DWS). :ref:`Table 1 <en-us_topic_0000001658895142__en-us_topic_0000001372679790_table19508017113430>` shows an example. For details about how to add an inbound rule, see "Security > Security Group > Adding a Security Group Rule" in the *Virtual Private Cloud User Guide*.

      .. _en-us_topic_0000001658895142__en-us_topic_0000001372679790_table19508017113430:

      .. table:: **Table 1** Inbound rule example

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Example Value                                                                                                                                                                            |
         +===================================+==========================================================================================================================================================================================+
         | Protocol/Application              | TCP                                                                                                                                                                                      |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Port                              | 8000                                                                                                                                                                                     |
         |                                   |                                                                                                                                                                                          |
         |                                   | .. note::                                                                                                                                                                                |
         |                                   |                                                                                                                                                                                          |
         |                                   |    Enter the value of **Database Port** set when creating the GaussDB(DWS) cluster. This port is used for receiving client connections to GaussDB(DWS). The default port number is 8000. |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Source                            | Select **IP address**, and enter the IP address and subnet mask of the client host that accesses GaussDB(DWS), for example, **192.168.0.10/16**.                                         |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  **EIP**: Select **Automatically assign** to apply for a cluster EIP as the public network IP address of the cluster. In addition, set the EIP bandwidth.


   .. figure:: /_static/images/en-us_image_0000001185831380.png
      :alt: **Figure 2** Configuring the network

      **Figure 2** Configuring the network

#. Configure the enterprise project to which the cluster belongs. You can configure this parameter only when the Enterprise Project Management service is enabled. The default value is **default**.

   An enterprise project facilitates project-level management and grouping of cloud resources and users.

   You can select the default enterprise project **default** or other existing enterprise projects. To create an enterprise project, log in to the Enterprise Management console. For details, see *Enterprise Management User Guide*.

#. Select **Default** for **Advanced Settings** in this example.

   -  **Default**: indicates that the following advanced settings use the default configurations.

      -  **CNs**: Three CNs are deployed by default.
      -  **Tag**: By default, no tag is added to the cluster.
      -  **Encrypt DataStore**: This parameter is disabled by default, indicating that the database is not encrypted.

   -  **Custom**: Select this option to configure the following advanced parameters: **Automated Snapshot**, **CNs**, **Tag**

#. Click **Create Now**. The **Confirm** page is displayed.

#. Click **Submit**.

   After the submission is successful, the creation starts. Click **Back to Cluster List**. The **Dedicated Clusters** page is displayed. The initial status of the cluster is **Creating**. Cluster creation takes some time. Wait for a while. Clusters in the **Available** state are ready for use.

.. |image1| image:: /_static/images/en-us_image_0000001231389491.png
