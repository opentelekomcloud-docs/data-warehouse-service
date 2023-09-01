:original_name: dws_01_00097.html

.. _dws_01_00097:

Restoring a Snapshot to the Original Cluster
============================================

Scenario
--------

You can use a snapshot to restore data to the original cluster. This function is used when a cluster is faulty or data needs to be rolled back to a specified snapshot version.

.. important::

   -  This function is supported only by clusters of version 8.1.3.200 or later.
   -  Snapshots whose backup device is OBS can be backed up.
   -  Only a snapshot in the **Available** state can be used for restoration.
   -  Logical clusters and resource pools cannot be restored to the current cluster.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Clusters**.

#. Click the name of a cluster and choose **Snapshots**.

#. Click **Restore**.

   |image1|

#. Restore the snapshot to the current cluster.

   |image2|

   .. note::

      If you use a snapshot to restore data to the original cluster, the cluster will be unavailable during the restoration.

.. |image1| image:: /_static/images/en-us_image_0000001517914161.png
.. |image2| image:: /_static/images/en-us_image_0000001466595234.png
