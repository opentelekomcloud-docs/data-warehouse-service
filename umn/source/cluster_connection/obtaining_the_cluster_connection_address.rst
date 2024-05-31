:original_name: dws_01_0033.html

.. _dws_01_0033:

Obtaining the Cluster Connection Address
========================================

Scenario
--------

You can access GaussDB(DWS) clusters by different methods and the connection address of each connection method varies. This section describes how to view and obtain the private network address on the cloud platform, public network address on the Internet, and JDBC connection strings.

To obtain the cluster connection address, use either of the following methods:

-  :ref:`Obtaining the cluster connection address on the Client Connections Page <en-us_topic_0000001658895162__en-us_topic_0000001423119209_section5539467151713>`
-  :ref:`Obtaining the Cluster Access Addresses on the Cluster Information Page <en-us_topic_0000001658895162__en-us_topic_0000001423119209_section149501253104810>`

.. _en-us_topic_0000001658895162__en-us_topic_0000001423119209_section5539467151713:

Obtaining the cluster connection address on the **Client Connections** Page
---------------------------------------------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, choose **Client Connections**.

#. In the **Data Warehouse Connection Information** area, select an available cluster.

   You can only select clusters in the **Available** state.

#. View and obtain the cluster connection information.

   -  **Private Network IP Address**
   -  **Public Network IP Address**
   -  **ELB Address**
   -  **JDBC URL (Private Network)**
   -  **JDBC URL (Public Network)**
   -  **ODBC URL**

   .. note::

      -  If no EIP is automatically assigned during cluster creation, **Public Network Address** is empty. If you want to use a public network address (consisting of an EIP and the database port) to access the cluster from the Internet, click **Bind EIP** to bind one.
      -  If an EIP is bound during cluster creation but you do not want to use the public network address to access the cluster, click **Unbind EIP** to unbind the EIP. After the EIP is unbound, **Public Network Address** is empty.

.. _en-us_topic_0000001658895162__en-us_topic_0000001423119209_section149501253104810:

Obtaining the Cluster Access Addresses on the Cluster Information Page
----------------------------------------------------------------------

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the **Connection** area, view and obtain the cluster's access address information, including the private network address and public network address.
