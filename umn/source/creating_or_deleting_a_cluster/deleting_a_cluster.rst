:original_name: dws_01_0025.html

.. _dws_01_0025:

Deleting a Cluster
==================

If you do not need to use a cluster, perform the operations in this section to delete it.

Impact on the System
--------------------

Deleted clusters cannot be recovered. Additionally, you cannot access user data and automated snapshots in a deleted cluster because the data and snapshots are automatically deleted. If you delete a cluster, its manual snapshots will not be deleted.


Deleting a Cluster
------------------

#. Log in to the GaussDB(DWS) console.

#. Click |image1| in the upper left corner of the management console to select a region.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be deleted.

#. In the row of a cluster, choose **More** > **Delete**.

#. In the displayed dialog box, confirm the deletion. You can determine whether to perform the following operations:

   -  Create a snapshot for the cluster.

      If the cluster status is normal, you can click **Create Snapshot**. In the dialog box that is displayed, enter the snapshot name and click **OK** to create a snapshot for the cluster to be deleted. In the row of a cluster, choose **More** > **Delete**.

   -  Resource

      -  Release the EIP bound to the cluster.

         If an EIP is bound to the cluster, you are advised to select **EIP** to release the EIP.

      -  Automated Snapshots

      -  Manual Snapshots

         If you have created a manual snapshot, you can select **Manual Snapshot** to delete it.

#. After confirming that the information is correct, enter **DELETE** or click **Auto Enter** and click **OK** to delete the cluster. The cluster status in the cluster list will change to **Deleting**, and the cluster deletion progress will be displayed.

   If the cluster to be deleted uses an automatically created security group that is not used by other clusters, the security group is automatically deleted when the cluster is deleted.

.. |image1| image:: /_static/images/en-us_image_0000001924569944.png
