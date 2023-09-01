:original_name: dws_01_0085.html

.. _dws_01_0085:

Copying Automated Snapshots
===========================

This section describes how to copy snapshots that are automatically created for long-term retention.

Copying an Automated Snapshot
-----------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane, choose **Snapshots**.

   All snapshots are displayed by default. You can copy the snapshots that were automatically created.

#. In the **Operation** column of the snapshot that you want to copy, choose **More** > **Copy**.

   -  **New Snapshot Name**: Enter a new snapshot name.

      The snapshot name must be 4 to 64 characters in length and start with a letter. It is case-insensitive and contains only letters, digits, hyphens (-), and underscores (_).

   -  **Snapshot Description**: Enter the snapshot information.

      This parameter is optional. Snapshot information contains 0 to 256 characters and does not support the following special characters: !<>'=&"


   .. figure:: /_static/images/en-us_image_0000001518033793.png
      :alt: **Figure 1** Copying a snapshot

      **Figure 1** Copying a snapshot

#. Click **OK**. The system starts to copy the snapshot for the cluster.

   The system displays a message indicating that the snapshot is successfully copied and delivered. After the snapshot is copied, the status of the copied snapshot is **Available**.

   .. note::

      If the snapshot size is much greater than that of the data stored in the cluster, the data is possibly labeled with a deletion tag, but is not cleared and reclaimed. In this case, clear the data and recreate a snapshot. For details, see :ref:`How Can I Clear and Reclaim the Storage Space? <dws_03_0033>`
