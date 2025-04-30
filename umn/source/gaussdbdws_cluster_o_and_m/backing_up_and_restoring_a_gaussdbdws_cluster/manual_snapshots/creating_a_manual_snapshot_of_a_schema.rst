:original_name: dws_01_1012.html

.. _dws_01_1012:

Creating a Manual Snapshot of a Schema
======================================

Overview
--------

A schema snapshot is a backup of specific schemas in a GaussDB(DWS) cluster at a specific point in time. This section describes how to create a schema snapshot on the **Snapshots** page.

A manual fine-grained snapshot can be created at any time. It will be retained until it is deleted from the GaussDB(DWS) console.

.. note::

   -  If the current console does not support this feature, contact technical support.

   -  Manual schema snapshots can be backed up to OBS or NFS.
   -  Schema snapshots can be created only for clusters in **Available** or **Unbalanced** state.

Prerequisites
-------------

Manually enable the fine-grained snapshot.

#. Choose **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page. Then, click **Snapshots**.

#. Click **Create Snapshot** in the upper right corner. Alternatively, choose **More** > **Create Snapshot** in the **Operation** column.

#. Click |image1| next to **Snapshot Level** and click **Set**.

   |image2|

#. On the **Snapshot List** page, toggle the fine-grained snapshot switch.

   |image3|: enabled

   |image4|: disabled

   |image5|

   .. note::

      -  If the fine-grained snapshot is enabled, you can restore specific tables from automatic or manual snapshots.

Impact on the System
--------------------

If a snapshot is being created for a cluster, the cluster cannot be restarted, scaled, its password cannot be reset, and its configurations cannot be modified.

.. note::

   To ensure the integrity of snapshot data, do not write data during snapshot creation.

Procedure
---------

#. Log in to the GaussDB(DWS) console.

#. Choose **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page. Then, click **Snapshots**.

#. Click **Create Snapshot** in the upper right corner. Alternatively, choose **More** > **Create Snapshot** in the **Operation** column.

#. Configure the following snapshot information:

   -  **Cluster Name**: Select a GaussDB(DWS) cluster from the drop-down list. The drop-down list only displays clusters that are in the **Available** state.
   -  **Snapshot Name**: Enter a snapshot name. The snapshot name must be 4 to 64 characters in length and start with a letter. It is case-insensitive and contains only letters, digits, hyphens (-), and underscores (_).
   -  **Snapshot Level**: Select **schema**.
   -  **Snapshot Description**: Enter the snapshot information. This parameter is optional. Snapshot information contains 0 to 256 characters and does not support the following special characters: !<>'=&"

#. Specify the snapshots to be backed up.

   -  Select a database from the **Database** drop-down list.
   -  In the schema list, select the schemas to be backed up. To search for a schema, enter its name in the search box in the upper right corner of the list, and click |image6|. Fuzzy search is supported.

   |image7|

   .. note::

      -  Schemas in different databases cannot be backed up at a time.
      -  By default, a maximum of 50 schemas can be backed up at a time.

#. Click **Create**.

   The task status of the cluster for which you are creating a snapshot is **Creating snapshot**. The status of the snapshot that is being created is **Creating**. After the snapshot is created, its status becomes **Available**.

   .. note::

      If a snapshot is larger than the available storage space in the cluster, check whether there is data that has been marked as deleted but actually still exists in the cluster. In this case, delete such data and create a snapshot again. For details, see :ref:`How Can I Clear and Reclaim the GaussDB(DWS) Storage Space? <dws_03_0033>`

.. |image1| image:: /_static/images/en-us_image_0000002167906512.png
.. |image2| image:: /_static/images/en-us_image_0000002203312733.png
.. |image3| image:: /_static/images/en-us_image_0000002203427205.png
.. |image4| image:: /_static/images/en-us_image_0000002167906520.png
.. |image5| image:: /_static/images/en-us_image_0000002168066216.png
.. |image6| image:: /_static/images/en-us_image_0000002203312729.jpg
.. |image7| image:: /_static/images/en-us_image_0000002203312753.png
