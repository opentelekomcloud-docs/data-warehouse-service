:original_name: dws_01_0033.html

.. _dws_01_0033:

Obtaining the Cluster Connection Address
========================================

Scenario
--------

You can access GaussDB(DWS) clusters by different methods and the connection address of each connection method varies. This section describes how to view and obtain the private network address on the cloud platform, public network address on the Internet, and JDBC connection strings.

To obtain the cluster connection address, use either of the following methods:

-  :ref:`Obtaining the Cluster Connection Address on the Connections Page <en-us_topic_0000001517355241__section5539467151713>`
-  :ref:`Obtaining the Cluster Access Addresses on the Cluster Information Page <en-us_topic_0000001517355241__section149501253104810>`

.. _en-us_topic_0000001517355241__section5539467151713:

Obtaining the Cluster Connection Address on the Connections Page
----------------------------------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **Connections**.

#. In the **Data Warehouse Connection Information** area, select an available cluster.

   You can only select clusters in the **Available** state.

#. View and obtain the cluster connection information.

   -  Private Network IP Address
   -  Public Network IP Address
   -  **ELB Address**
   -  **JDBC URL (Private Network)**
   -  **JDBC URL (Public Network)**
   -  **ODBC URL**

   .. note::

      -  If no EIP is automatically assigned during cluster creation, **Public Network IP Address** is empty. If you want to use a public network address (consisting of an EIP and the database port) to access the cluster from the Internet, click **Bind EIP** to bind one.
      -  If an EIP is bound during cluster creation but you do not want to use the public network address to access the cluster, click **Unbind EIP** to unbind the EIP. After the EIP is unbound, **Public Network IP Address** is empty.

.. _en-us_topic_0000001517355241__section149501253104810:

Obtaining the Cluster Access Addresses on the Cluster Information Page
----------------------------------------------------------------------

#. Log in to the GaussDB(DWS) management console.
#. In the navigation tree on the left, click **Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the **Connection** area, view and obtain the cluster's access address information, including the private network address and public network address.

   .. table:: **Table 1** Connection

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                               |
      +===================================+===========================================================================================================================================================================================================================================+
      | Private Network Domain Name       | Name of the domain for accessing the database in the cluster over the private network. The private network domain address is automatically generated when a cluster is created. The default naming rule is *cluster name*.dws.otc-tsi.de. |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   | .. note::                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   |    If the cluster name does not comply with the domain name standards, the prefix of the default access domain name will be adjusted accordingly.                                                                                         |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   | You can click **Modify** to change the private network domain name. The access domain name contains 4 to 63 characters, which consists of letters, digits, and hyphens (-), and must start with a letter.                                 |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   | For details, see :ref:`Managing Access Domain Names <dws_01_0140>`.                                                                                                                                                                       |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Private Network IP Address        | IP address for accessing the database in the cluster over the private network.                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   | .. note::                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   |    -  A private IP address is automatically generated when you create a cluster. The IP address is fixed.                                                                                                                                 |
      |                                   |    -  The number of private IP addresses equals the number of CNs. You can log in to any node to connect to the cluster.                                                                                                                  |
      |                                   |    -  If you access a fixed IP address over the internal network, all the resource pools will run on a single CN.                                                                                                                         |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Public Network Domain Name        | Name of the domain for accessing the database in the cluster over the public network.                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   | For details, see :ref:`Managing Access Domain Names <dws_01_0140>`.                                                                                                                                                                       |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Public Network IP Address         | IP address for accessing the database in the cluster over the public network.                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   | .. note::                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                           |
      |                                   |    -  If no EIP is assigned during cluster creation and **Public Network IP Address** is empty, click **Edit** to bind an EIP to the cluster.                                                                                             |
      |                                   |    -  If an EIP is bound during cluster creation, click **Edit** to unbind the EIP.                                                                                                                                                       |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Initial Administrator             | Database administrator specified during cluster creation. When you connect to the cluster for the first time, you need to use the initial database administrator and password to connect to the default database.                         |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Port                              | Port for accessing the cluster database over the public network or private network. The host port is specified when a cluster is created and used to listen to client connections.                                                        |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Default Database                  | Database name specified when the cluster is created. When you connect to the cluster for the first time, connect to the default database.                                                                                                 |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ELB Address                       | To achieve high availability and avoid single-CN failures, a new cluster needs to be bound to ELB. You are advised to use the ELB address to connect to the cluster.                                                                      |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
