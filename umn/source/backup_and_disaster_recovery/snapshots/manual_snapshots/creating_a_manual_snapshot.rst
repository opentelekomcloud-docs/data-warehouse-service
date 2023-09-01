:original_name: dws_01_0028.html

.. _dws_01_0028:

Creating a Manual Snapshot
==========================

Prerequisites
-------------

A cluster snapshot is a complete backup that records point-in-time configuration data and service data of a GaussDB(DWS) cluster. This section describes how to create a snapshot on the **Snapshots** page to back up cluster data.

A manual snapshot can be created at any time. It will be retained until it is deleted from the GaussDB(DWS) console. Manual snapshots are full backup data, which takes a long time to create.

.. note::

   -  Manual cluster snapshots can be backed up to OBS.
   -  To create a manual snapshot of a cluster, the cluster state must be **Available**, **To be restarted**, or **Unbalanced**. In cluster versions earlier than 8.1.3.101, you can also create a snapshot of a cluster in the **Read-only** state.

Impact on the System
--------------------

If a snapshot is being created for a cluster, the cluster cannot be restarted, scaled, its password cannot be reset, and its configurations cannot be modified.

.. note::

   To ensure the integrity of snapshot data, do not write data during snapshot creation.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane, choose **Snapshots**. Click **Create Snapshot** in the upper right corner. Alternatively, choose **More** > **Create Snapshot** in the **Operation** column.

   |image1|

   |image2|

#. Configure the following snapshot information:

   -  **Cluster Name**: Select a GaussDB(DWS) cluster from the drop-down list. The drop-down list only displays clusters that are in the **Available** state.
   -  **Snapshot Name**: Enter a snapshot name. The snapshot name must be 4 to 64 characters in length and start with a letter. It is case-insensitive and contains only letters, digits, hyphens (-), and underscores (_).
   -  **Snapshot Level**: Select **cluster**.
   -  **Snapshot Description**: Enter the snapshot information. This parameter is optional. Snapshot information contains 0 to 256 characters and does not support the following special characters: !<>'=&"

   |image3|

#. Click **Create**.

   Task status of the cluster for which you are creating a snapshot is **Creating snapshot**. The status of the snapshot that is being created is **Creating**. After the snapshot is created, its status becomes **Available**.

   .. note::

      If the snapshot size is much greater than that of the data stored in the cluster, the data is possibly labeled with a deletion tag, but is not cleared and reclaimed. In this case, clear the data and recreate a snapshot. For details, see :ref:`How Can I Clear and Reclaim the Storage Space? <dws_03_0033>`

.. |image1| image:: /_static/images/en-us_image_0000001518033881.png
.. |image2| image:: /_static/images/en-us_image_0000001467074206.png
.. |image3| image:: /_static/images/en-us_image_0000001466754718.png
