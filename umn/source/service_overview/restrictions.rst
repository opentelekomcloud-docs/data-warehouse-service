:original_name: dws_01_0010.html

.. _dws_01_0010:

Restrictions
============

-  You can manage clusters only and cannot directly access nodes in a cluster. You can use a cluster's IP address and port to access the database in the cluster.
-  Currently, you can only modify the specifications of standard data warehouse clusters and stream data warehouse clusters that only use ECS and EVS resources for computing and storage. If your cluster contains other computing or storage resources but you want to change to a higher node flavor, create a new cluster.
-  If you use a client to connect to a cluster, its VPC subnet must be the same as that of the cluster.
-  If you copy commands from the document to the operating environment, the text wraps automatically, causing command execution failures. To solve the problem, delete the line break.
