:original_name: dws_01_0028.html

.. _dws_01_0028:

Manually Creating a Snapshot
============================

Prerequisites
-------------

A snapshot is a complete backup that records point-in-time configuration data and service data of a GaussDB(DWS) cluster. This section describes how to create a snapshot on the **Snapshots** page to back up cluster data.

A manual snapshot can be created at any time. It will be retained until it is deleted from the GaussDB (DWS) console. Manual snapshots are full backups, which takes a long time to create.

.. note::

   -  Manual snapshots can be backed up to OBS.
   -  Snapshots can be created only for clusters in **Available**, **Read-only**, or **Unbalanced** state.

Impact on the System
--------------------

If a snapshot is being created for a cluster, the cluster cannot be restarted, scaled, its password cannot be reset, and its configurations cannot be modified.

.. note::

   To ensure the integrity of snapshot data, do not write data during snapshot creation.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane, choose **Snapshots**.

#. Click **Create Snapshot** and enter snapshot information.

   -  **Cluster Name**: Select a GaussDB(DWS) cluster from the drop-down list. The drop-down list only displays clusters that are in the **Available** state.
   -  **Snapshot Name**: Enter a snapshot name. The snapshot name must be 4 to 64 characters in length and start with a letter. It is case-insensitive and contains only letters, digits, hyphens (-), and underscores (_).
   -  **Snapshot Description**: Enter the snapshot information. This parameter is optional. Snapshot information contains 0 to 256 characters and does not support the following special characters: !<>'=&"


   .. figure:: /_static/images/en-us_image_0000001231733612.png
      :alt: **Figure 1** Creating a snapshot

      **Figure 1** Creating a snapshot

#. Click **OK**.

   The task status of the cluster for which you are creating a snapshot is **Creating snapshot**. The status of the snapshot that is being created is **Creating**. After the snapshot is created, its status becomes **Available**.

   .. note::

      If the snapshot size is much greater than that of the data stored in the cluster, the data is possibly labeled with a deletion tag, but is not cleared and reclaimed. Clear the data and recreate a snapshot. For details, see :ref:`How Can I Clear and Reclaim Storage? <dws_03_0033>`
