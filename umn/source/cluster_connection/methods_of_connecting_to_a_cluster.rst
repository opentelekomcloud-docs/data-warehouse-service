:original_name: dws_01_0137.html

.. _dws_01_0137:

Methods of Connecting to a Cluster
==================================

If you have created a GaussDB(DWS) cluster, you can use the SQL client tool or a third-party driver such as JDBC or ODBC to connect to the cluster and access the database in the cluster.

The procedure for connecting to a cluster is as follows:

#. :ref:`Obtaining the Cluster Connection Address <dws_01_0033>`
#. If SSL encryption is used, perform the operations in :ref:`Establishing Secure TCP/IP Connections in SSL Mode <dws_01_0038>`.
#. Connect to the cluster and access the database in the cluster. You can choose any of the following methods to connect to a cluster:

   -  Use the SQL client tool to connect to the cluster.

      -  :ref:`Using the Linux gsql Client to Connect to a Cluster <dws_01_0037>`
      -  :ref:`Using the Windows gsql Client to Connect to a Cluster <dws_01_0128>`
      -  :ref:`Using the Data Studio GUI Client to Connect to a Cluster <dws_01_0094>`

   -  Use a JDBC, psycopg2, or PyGreSQL driver to connect to the cluster.

      -  :ref:`Using JDBC to Connect to a Cluster <dws_01_0077>`
      -  :ref:`Using ODBC to Connect to a Cluster <dws_01_0086>`
      -  :ref:`Using the Third-Party Function Library psycopg2 of Python to Connect to a Cluster <dws_01_0120>`
      -  :ref:`Using the Python Library PyGreSQL to Connect to a Cluster <dws_01_0171>`
      -  :ref:`Using JDBC to Connect to a Cluster (IAM Authentication Mode) <dws_01_0133>`
