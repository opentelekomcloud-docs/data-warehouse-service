:original_name: dws_01_0030.html

.. _dws_01_0030:

Deleting a Manual Snapshot
==========================

On the **Snapshot Management** page of the GaussDB(DWS) console, you can delete an unwanted snapshot in the **Unavailable** state or delete an available snapshot to release the storage space.

.. caution::

   Deleted snapshots cannot be recovered. Exercise caution when performing this operation.

Procedure
---------

#. Log in to the GaussDB(DWS) console.
#. Choose **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page. Then, click **Snapshots**. All snapshots are displayed by default.
#. In the **Operation** column of the snapshot that you want to delete, choose **More** > **Delete**.

   .. note::

      You can only delete snapshots that were manually created.

#. If the information is correct, enter **DELETE** and click **OK** to delete the snapshot.
