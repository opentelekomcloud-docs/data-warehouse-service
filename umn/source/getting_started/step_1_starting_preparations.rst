:original_name: dws_01_0088.html

.. _dws_01_0088:

Step 1: Starting Preparations
=============================

This guide is an introductory tutorial that demonstrates how to create a sample GaussDB(DWS) cluster, connect to the cluster database, import the sample data from OBS, and analyze the sample data. You can use this tutorial to evaluate GaussDB(DWS).

Before creating a GaussDB(DWS) cluster, ensure that the following prerequisites are met:

-  :ref:`Determining the Cluster Ports <en-us_topic_0000001518033685__section788591414617>`

.. _en-us_topic_0000001518033685__section788591414617:

Determining the Cluster Ports
-----------------------------

-  When creating a GaussDB(DWS) cluster, you need to specify a port for SQL clients or applications to access the cluster.
-  If your client is behind a firewall, you need an available port so that you can connect to the cluster and perform query and analysis from the SQL client tool.
-  If you do not know an available port, contact the network administrator to specify an open port on your firewall. The ports supported by GaussDB(DWS) range from 8000 to 30000.
-  After a cluster is created, its port number cannot be changed. Ensure that the port specified is available.
