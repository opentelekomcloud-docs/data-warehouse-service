:original_name: dws_01_0073.html

.. _dws_01_0073:

Managing Clusters That Fail to Be Created
=========================================

If a cluster fails to be created, you can go to the **Clusters** page of the GaussDB(DWS) management console to view the cluster status and the cause of failure.

Checking the Cause of a Creation Failure
----------------------------------------

#. Log in to the GaussDB(DWS) management console and click **Clusters** in the left navigation pane on the left.

#. In the cluster list, locate the cluster whose **Cluster Status** is **Creation failed**.

#. Click |image1| in the **Cluster Status** column to view the cause of the creation failure.

   For details about the error code and how to handle it, see *Error Code Reference*. If the fault persists, contact technical support.

Deleting a Cluster That Fails to Be Created
-------------------------------------------

You can delete a cluster that fails to be created if you do not need it. Before deletion, check the cause of creation failure.

#. Log in to the GaussDB(DWS) management console and click **Clusters** in the left navigation pane on the left.

#. In the cluster list, locate the row containing the failed cluster to be deleted, and choose **More** > **Delete**.

#. (Optional) If the cluster is bound with an EIP during creation, click **Release the EIP bound with the cluster** to release the EIP.

#. In the dialog box that is displayed, click **Yes** to delete the cluster.

   If the cluster to be deleted uses an automatically created security group that is not used by other clusters, the security group is automatically deleted when the cluster is deleted.

.. |image1| image:: /_static/images/en-us_image_0000001466914218.png
