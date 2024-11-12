:original_name: dws_01_0154.html

.. _dws_01_0154:

Preparations
============

Before using GaussDB(DWS), make the following preparations:

-  :ref:`Determining the Cluster Ports <en-us_topic_0000001952008053__section788591414617>`

.. _en-us_topic_0000001952008053__section788591414617:

Determining the Cluster Ports
-----------------------------

-  When creating a GaussDB(DWS) cluster, you need to specify a port for SQL clients or applications to access the cluster.
-  If your client is behind a firewall, you need an available port so that you can connect to the cluster and perform query and analysis from the SQL client tool.
-  If you do not know an available port, contact the network administrator to specify an open port on your firewall. GaussDB(DWS) supports port numbers between **8000** and **30000**. By default, a data warehouse cluster is created with port number **8000**.
-  After a cluster is created, its port number cannot be changed. Ensure that the port specified is available.
