:original_name: dws_01_0010.html

.. _dws_01_0010:

Restrictions
============

-  You can manage clusters only and cannot directly access nodes in a cluster. You can use a cluster's IP address and port to access the database in the cluster.
-  You cannot change the flavor of an existing cluster. If you need nodes with a higher flavor, create a new one.
-  If you use a client to connect to a cluster, its VPC subnet must be the same as that of the cluster.
-  If you copy commands from the document to the operating environment, the text wraps automatically, causing command execution failures. To solve the problem, delete the line break.
