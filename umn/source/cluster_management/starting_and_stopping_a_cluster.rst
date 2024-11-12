:original_name: dws_01_0024.html

.. _dws_01_0024:

Starting and Stopping a Cluster
===============================

Restarting a cluster
--------------------

If a cluster is in the **Unbalanced** state or cannot work properly, you may need to restart it for restoration. After modifying a cluster's configurations, such as security settings and parameters, manually restart the cluster to make the configurations take effect.

**Impact on the System**

-  A cluster cannot provide services during the restart. Therefore, before the restart, ensure that no task is running and all data is saved.

   If the cluster is processing service data, such as importing data, querying data, creating snapshots, or restoring snapshots, cluster restarting will cause file damage or restart failure. You are advised to stop all cluster tasks before restarting the cluster.

   View the **Session Count** and **Active SQL Count** metrics to check whether the cluster has active events. For details, see :ref:`Monitoring Clusters Using Cloud Eye <dws_01_0022>`.

-  The time required for restarting a cluster depends on the cluster scale and services. Generally, it takes about 3 minutes to restart a cluster. The duration does not exceed 20 minutes.

-  If the restart fails, the cluster may be unavailable. Try again later or contact technical support.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters** > **Dedicated Clusters**.

#. In the **Operation** column of the cluster to be restarted, click **Restart**.

#. In the dialog box that is displayed, click **Yes**.

   **Task Information** changes to **Restarting**. When **Cluster Status** changes to **Available** again, the cluster is successfully restarted.

Stopping a Cluster
------------------

If a cluster is no longer used, you can stop the cluster to bring services offline.

.. note::

   -  If the current console does not support this feature, contact technical support.
   -  After the cluster is stopped, ECS basic resources (vCPUs and memory) are no longer reserved. When you start the service again, it may fail to be started due to insufficient resources. In this case, wait for a while and try again later.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. On the **Dedicated Clusters** page, locate the row that contains the target dedicated cluster, click **More** > **Stop** in the **Operation** column.

#. In the dialog box that is displayed, click **Yes**.

   The **Task Information** of the cluster changes to **Stopping**. If the **Cluster Status** changes to **Stopped**, the cluster is stopped successfully.

Starting a Cluster
------------------

You can start a stopped cluster to restore cluster services.

.. note::

   If the current console does not support this feature, contact technical support.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. On the **Dedicated Clusters** page, locate the row that contains the target dedicated cluster, click **More** > **Start** in the **Operation** column.

#. In the dialog box that is displayed, click **Yes**.

   The **Task Information** of the cluster changes to **Starting**. If the **Cluster Status** changes to **Available**, the cluster is started successfully.
