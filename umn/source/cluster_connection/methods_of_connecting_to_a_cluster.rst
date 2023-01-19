:original_name: dws_01_0137.html

.. _dws_01_0137:

Methods of Connecting to a Cluster
==================================

If you have created a GaussDB(DWS) cluster, you can use the SQL client tool or a third-party driver such as JDBC or ODBC to connect to the cluster and access the database in the cluster.

The procedure for connecting to a cluster is as follows:

#. :ref:`Obtaining the Cluster Connection Address <dws_01_0033>`
#. If SSL encryption is used, perform the following steps:

   a. :ref:`(Optional) Configuring SSL Connection <dws_01_0076>`
   b. :ref:`(Optional) Downloading the SSL Certificate <dws_01_0083>`

#. Connect to the cluster and access the database in the cluster. You can choose any of the following methods to connect to a cluster:

   -  Use the SQL client tool to connect to the cluster.

      -  :ref:`Using the gsql Client to Connect to a Cluster <dws_01_0037>`
      -  :ref:`Using the Data Studio GUI Client to Connect to a Cluster <dws_01_0094>`

   -  Use a JDBC or ODBC driver to connect to the cluster.

      -  :ref:`Using a JDBC Driver to Connect to a Database <dws_01_0077>`
      -  :ref:`Using an ODBC Driver to Connect to a Database <dws_01_0086>`
      -  :ref:`Configuring the JDBC Connection to Connect to a Cluster Using IAM Authentication <dws_01_0134>`
