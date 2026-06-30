:original_name: dws_01_0137.html

.. _dws_01_0137:

Overview
========

If you have created a DWS cluster, you can use the SQL client tool or a third-party driver such as JDBC or ODBC to connect to the cluster and access the database in the cluster.

-  You are advised to use the officially recommended method for connecting to the database.
-  Compatibility with other clients cannot be guaranteed, so it may be necessary to verify it.
-  If an error occurs due to incompatibility with another client and the client cannot be replaced, try replacing the libpq driver on the client. To replace the **libpg.so** file on the client, download and extract the gsql client package by referring to :ref:`Downloading the Client <dws_01_0031>`, locate the **gsql** directory, and obtain the file. Then, replace the existing **libpg.so** file in the designated directory on the client.

Constraints and Limitations
---------------------------

-  Avoid having all business operations run under a single database user. Instead, plan different database users according to the business modules.
-  For better access control of different business modules, it is better to use multiple users and permissions instead of depending on the system administrator user to run business operations.
-  You are not advised to connect services to a single CN. Instead, configure load balancing to ensure that connections to each CN are balanced.
-  After connecting to the database and completing required operations, close the database connection in a timely manner to prevent idle connections from continuously occupying resources and consuming connections and public resources.
-  In the scenario where the database connection pool is used, after the database GUC parameters are set using the **SET** statement in the service, the parameters must be restored using the **RESET** statement before the connection pool is returned.
-  For more information about development and design specifications, see "DWS Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Connecting to a Cluster
-----------------------

The procedure for connecting to a cluster is as follows:

#. :ref:`Obtain the connection address of a DWS cluster. <dws_01_0033>`
#. If SSL encryption is used, perform the operations in :ref:`Establishing Secure TCP/IP Connections in SSL Mode <dws_01_0038>`.
#. Connect to the cluster and access the database in the cluster. You can choose any of the following methods to connect to a cluster:

   -  Use the SQL client tool to connect to the cluster.

      -  :ref:`Using the Linux gsql Client to Connect to a Cluster <dws_01_0037>`
      -  :ref:`Using the Windows gsql Client to Connect to a Cluster <dws_01_0128>`
      -  :ref:`Using Data Studio to Connect to a DWS Cluster <dws_01_0094>`

   -  Use the JDBC, ODBC, psycopg2, or PyGreSQL third-party driver to connect to a cluster.

      -  :ref:`Using JDBC to Connect to a Cluster <dws_01_0077>`
      -  :ref:`Using ODBC to Connect to a Cluster <dws_01_0086>`
      -  :ref:`Using the Python Library psycopg2 to Connect to a DWS Cluster <dws_01_0120>`
      -  :ref:`Using the Python Library PyGreSQL to Connect to a DWS Cluster <dws_01_0171>`
      -  :ref:`Configuring JDBC to Connect to a Cluster (IAM Authentication Mode) <dws_01_0133>`
