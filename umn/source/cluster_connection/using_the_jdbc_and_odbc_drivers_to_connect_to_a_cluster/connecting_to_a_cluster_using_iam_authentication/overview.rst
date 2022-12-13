:original_name: dws_01_0134.html

.. _dws_01_0134:

Overview
========

GaussDB(DWS) allows you to access databases using IAM authentication. When you use the JDBC application program to connect to a cluster, set the IAM username, credential, and other information as you configure the JDBC URL. After doing this, when you try to access a database, the system will automatically generate a temporary credential and a connection will be set up.

.. note::

   -  Currently, only clusters 1.3.1 and later versions and their corresponding JDBC drivers can access the databases in IAM authentication mode.

IAM supports two types of user credential: password and Access Key ID/Secret Access Key (AK/SK). JDBC connection requires the latter.

The IAM account you use to access a database must be granted with the **DWS Database Access** permission. Only users with both the **DWS Administrator** and **DWS Database Access** permissions can connect to GaussDB(DWS) databases using the temporary database user credentials generated based on IAM users.

The process of accessing a database is as follows:

#. :ref:`Granting an IAM Account the DWS Database Access Permission <dws_01_0135>`
#. :ref:`Creating an IAM User Credential <dws_01_0136>`
#. :ref:`Configuring the JDBC Connection to Connect to a Cluster Using IAM Authentication <dws_01_0132>`
