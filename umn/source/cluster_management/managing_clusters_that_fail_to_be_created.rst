:original_name: dws_01_0073.html

.. _dws_01_0073:

Managing Clusters That Fail to Be Created
=========================================

If a cluster fails to be created, you can go to the **Clusters** > **Dedicated Clusters** page of the GaussDB(DWS) console to view the cluster status and the cause of failure.

Checking the Cause of a Creation Failure
----------------------------------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, locate the cluster whose **Cluster Status** is **Creation failed**.

#. Click |image1| in the **Cluster Status** column to view the cause of the creation failure.

   You can rectify the fault based on the error code displayed. For details, see Error Code Reference. If the fault persists, contact technical support.

Deleting a Cluster That Fails to Be Created
-------------------------------------------

You can delete a cluster that fails to be created if you do not need it. Before deletion, check the cause of creation failure.

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, locate the row containing the failed cluster to be deleted, and choose **More** > **Delete**.

#. In the displayed dialog box, confirm the deletion. You can determine whether to perform the following operations:

   -  Create a snapshot for the cluster.

      If the cluster status is normal, you can click **Create Snapshot**. In the dialog box that is displayed, enter the snapshot name and click **OK** to create a snapshot for the cluster to be deleted. In the row of a cluster, choose **More** > **Delete**.

   -  Delete associated resources:

      -  Release the EIP bound to the cluster.

         If an EIP is bound to the cluster, you are advised to select **EIP** to release the EIP.

      -  Delete automated snapshots.

      -  Delete manual snapshots.

         If you have created a manual snapshot, you can select **Manual Snapshot** to delete it.

#. After confirming that the information is correct, enter **DELETE** or click **Auto Enter** and click **OK** to delete the cluster. The cluster status in the cluster list will change to **Deleting**, and the cluster deletion progress will be displayed.

   If the cluster to be deleted uses an automatically created security group that is not used by other clusters, the security group is automatically deleted when the cluster is deleted.

.. |image1| image:: /_static/images/en-us_image_0000001924728892.png
