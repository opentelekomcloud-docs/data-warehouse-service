:original_name: dws_01_0024.html

.. _dws_01_0024:

Starting, Stopping, and Deleting a GaussDB(DWS) Cluster
=======================================================

.. _en-us_topic_0000002203312245__section215618565260:

Restarting a cluster
--------------------

If a cluster is in the **Unbalanced** state or cannot work properly, you may need to restart it for restoration. After modifying a cluster's configurations, such as security settings and parameters, manually restart the cluster to make the configurations take effect.

**Impact on the System**

-  A cluster cannot provide services during the restart. Therefore, before the restart, ensure that no task is running and all data is saved.

   If the cluster is processing service data, such as importing data, querying data, creating snapshots, or restoring snapshots, cluster restarting will cause file damage or restart failure. You are advised to stop all cluster tasks before restarting the cluster.

   View the **Session Count** and **Active SQL Count** metrics to check whether the cluster has active events. For details, see :ref:`Viewing GaussDB(DWS) Cluster Monitoring Information on Cloud Eye <dws_01_0022>`.

-  The time required for restarting a cluster depends on the cluster scale and services. Generally, it takes about 3 minutes to restart a cluster. The duration does not exceed 20 minutes.

-  If the restart fails, the cluster may be unavailable. Try again later or contact technical support.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**.

#. In the **Operation** column of the cluster to be restarted, choose **More** > **Restart**.

   .. note::

      The positions of the function keys in the **Operation** column are dynamic. To ensure that there are always two function keys visible before **More**, any function keys that typically appear only when you hover over **More** will be moved to a position directly before **More**. This adjustment occurs if there are some functions whose keys are supposed to be placed before **More** but are not supported for the current site.

#. In the dialog box that is displayed, click **Yes**.

   **Task Information** changes to **Restarting**. When **Cluster Status** changes to **Available** again, the cluster is successfully restarted.

.. _en-us_topic_0000002203312245__section193983556288:

Stopping a Cluster
------------------

If a cluster is no longer used, you can stop the cluster to bring services offline.

.. note::

   -  If the current console does not support this feature, contact technical support.
   -  After the cluster is stopped, ECS basic resources (vCPUs and memory) are no longer reserved. When you start the service again, it may fail to be started due to insufficient resources. In this case, wait for a while and try again later.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. On the **Dedicated Clusters** page, locate the row that contains the target dedicated cluster, click **More** > **Stop** in the **Operation** column.

#. In the dialog box that is displayed, click **Yes**.

   The **Task Information** of the cluster changes to **Stopping**. If the **Cluster Status** changes to **Stopped**, the cluster is stopped successfully.

.. _en-us_topic_0000002203312245__section6834151811262:

Starting a Cluster
------------------

You can start a stopped cluster to restore cluster services.

.. note::

   If the current console does not support this feature, contact technical support.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. On the **Dedicated Clusters** page, locate the row that contains the target dedicated cluster, click **More** > **Start** in the **Operation** column.

#. In the dialog box that is displayed, click **Yes**.

   The **Task Information** of the cluster changes to **Starting**. If the **Cluster Status** changes to **Available**, the cluster is started successfully.

Deleting a Cluster
------------------

If you do not need to use a cluster, perform the operations in this section to delete it.

**Impact on the System**

Deleted clusters cannot be recovered. Additionally, you cannot access user data and automated snapshots in a deleted cluster because the data and snapshots are automatically deleted. If you delete a cluster, its manual snapshots will not be deleted.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Click |image1| in the upper left corner of the management console to select a region.

#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be deleted.

#. In the row of a cluster, choose **More** > **Delete**.

#. In the displayed dialog box, confirm the deletion. You can determine whether to perform the following operations:

   -  Create a snapshot for the cluster.

      If the cluster status is normal, click **Create Snapshot**. On the snapshot list page, click **Create Snapshot** to create a snapshot for the cluster to be deleted. For details, see :ref:`Manual Snapshots <dws_01_0092>`. In the row of a cluster, choose **More** > **Delete**.

   -  Delete associated resources.

      -  Release the EIP bound to a cluster.

         If an EIP is bound to the cluster, you are advised to select **EIP** to release the EIP of the cluster to be deleted.

      -  Delete automated snapshots.

      -  Delete manual snapshots.

         If you have created a manual snapshot, you can select **Manual Snapshot** to delete it.

#. After confirming that the information is correct, enter **DELETE** or click **Auto Enter** and click **OK** to delete the cluster. The cluster status in the cluster list will change to **Deleting** and the cluster deletion progress will be displayed.

   If the cluster to be deleted uses an automatically created security group that is not used by other clusters, the security group is automatically deleted when the cluster is deleted.

.. |image1| image:: /_static/images/en-us_image_0000002168066232.png
