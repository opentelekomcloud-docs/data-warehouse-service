:original_name: dws_01_0088.html

.. _dws_01_0088:

Step 1: Starting Preparations
=============================

This guide is an introductory tutorial that demonstrates how to create a sample GaussDB(DWS) cluster, connect to the cluster database, import the sample data from OBS, and analyze the sample data. You can use this tutorial to evaluate GaussDB(DWS).

Before creating a GaussDB(DWS) cluster, ensure that the following prerequisites are met:

-  :ref:`Registering a Public Cloud Account and Completing Real-Name Authentication <en-us_topic_0000001134400758__section269013516364>`
-  :ref:`Determining the Cluster Ports <en-us_topic_0000001134400758__section788591414617>`

.. _en-us_topic_0000001134400758__section269013516364:

Registering a Public Cloud Account and Completing Real-Name Authentication
--------------------------------------------------------------------------

If you do not have a public cloud account, register one. If you already have an account with real-name authentication, skip this step and use the account.

#. Open the official public cloud website and click **Register** in the upper right corner. The registration page is displayed.
#. Fill in user information as instructed to complete the registration.
#. Click the username in the upper right corner to enter the **Account Info** page. Then click **Real-Name Authentication** in the left navigation pane.
#. Perform real-name authentication as prompted.

   .. note::

      You must perform real-name authentication before enabling cloud services.

.. _en-us_topic_0000001134400758__section788591414617:

Determining the Cluster Ports
-----------------------------

-  When creating a GaussDB(DWS) cluster, you need to specify a port for SQL clients or applications to access the cluster.
-  If your client is behind a firewall, you need an available port so that you can connect to the cluster and perform query and analysis from the SQL client tool.
-  If you do not know an available port, contact the network administrator to specify an open port on your firewall. The ports supported by GaussDB(DWS) range from 8000 to 30000.
-  After a cluster is created, its port number cannot be changed. Ensure that the port specified is available.
