:original_name: dws_01_0024.html

.. _dws_01_0024:

Cluster Restart
===============

If a cluster is in the **Unbalanced** state or cannot work properly, you may need to restart it for restoration. After modifying a cluster's configurations, such as security settings and parameters, manually restart the cluster to make the configurations take effect.

Impact on the System
--------------------

-  A cluster cannot provide services during the restart. Therefore, before the restart, ensure that no task is running and all data is saved.

   If the cluster is processing service data, such as importing data, querying data, creating snapshots, or restoring snapshots, cluster restarting will cause file damage or restart failure. You are advised to stop all cluster tasks before restarting the cluster.

   View the **Session Count** and **Active SQL Count** metrics to check whether the cluster has active events. For details, see :ref:`Monitoring Clusters Using Cloud Eye <dws_01_0022>`.

-  The time required for restarting a cluster depends on the cluster scale and services. Generally, it takes about 3 minutes to restart a cluster. The duration does not exceed 20 minutes.

-  If the restart fails, the cluster may be unavailable. Try again later or contact technical support.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**.

#. In the **Operation** column of the cluster to be restarted, click **Restart**.

#. In the dialog box that is displayed, click **Yes**.

   **Task Information** changes to **Restarting**. When **Cluster Status** changes to **Available** again, the cluster is successfully restarted.
