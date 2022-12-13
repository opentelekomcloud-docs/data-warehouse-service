:original_name: dws_01_0023.html

.. _dws_01_0023:

Cluster Scale-out
=================

As your data warehouse capacity and performance requirements change, you can adjust the sizes of existing clusters on the management console to utilize compute and storage resources provided by GaussDB(DWS).

After the data in a data warehouse is deleted, the occupied disk space may not be released, resulting in dirty data and disk waste. Therefore, if you need to scale out your cluster due to insufficient storage capacity, run the **VACUUM** command to reclaim the storage space first. If the used storage capacity is still high after you run the **VACUUM** command, you can scale out your cluster. For details about **VACUUM**, see "SQL Reference > SQL Syntax > VACUUM" in the *Data Warehouse Service (DWS) Developer Guide*.

Impact on the System
--------------------

-  Before the scale-out, exit the client connections that have created temporary tables because temporary tables created before or during the scale-out will become invalid and operations performed on these temporary tables will fail. Temporary tables created after the scale-out will not be affected.
-  During the scale-out, you cannot restart or scale a cluster, create a snapshot, reset a password, or delete a cluster.
-  During the scale-out, the cluster automatically restarts. Therefore, the cluster stays **Unavailable** for a period of time. After the cluster is restarted, the status becomes **Available**. After scale-out, the system dynamically redistributes user data among all nodes in the cluster.

Prerequisites
-------------

-  The cluster to be scaled out is in the **Available** or **Unbalanced** state.
-  The number of nodes to be added must be less than or equal to the available nodes. Otherwise, system scale-out is not allowed.

Scaling Out a Cluster
---------------------

.. note::

   -  A cluster becomes read-only during scale-out. Exercise caution when performing this operation.
   -  To ensure data security, create a manual snapshot before scaling. For details about how to create a snapshot, see :ref:`Manually Creating a Snapshot <dws_01_0028>`.

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**.

   All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Out**.

   On the page that is displayed, the **Automated Backup** button is enabled by default.

#. Specify the number of nodes to be added.

   -  The number of nodes after scale-out must be at least three nodes more than the original number. The maximum number of nodes that can be added depends on the available quota. In addition, the number of nodes after the scale-out cannot exceed 256.

      If the node quota is insufficient, click **Increase quota** to submit a service ticket and apply for higher node quota.

   -  Flavor of the new nodes must be the same as that of existing nodes in the cluster.

   -  The VPC, subnet, and security group of the cluster with new nodes added are the same as those of the original cluster.

#. .. _en-us_topic_0000001180320133__li1283703664815:

   Configure advanced parameters. If you choose **Custom**, you can enable **Online Scale-out** and **Auto Redistribution**, and set **Redistribution Mode** to **Online**. Click **OK** if a message is prompted.

   If you choose **Default**, **Online Scale-out** will be disabled, **Auto Redistribution** will be enabled, and **Redistribution Mode** will be **Offline** by default.

#. Click **Next: Confirm**.

#. Click **Scale Out Now**.

   .. note::

      If the number of requested nodes, vCPU (cores), or memory (GB) exceed the user's remaining quota, a warning dialog box is displayed, indicating that the quota is insufficient and displaying the detailed remaining quota and the current quota application. You can click **Increase quota** in the warning dialog box to submit a service ticket and apply for higher node quota.

      For details about quotas, see :ref:`What Is the User Quota? <dws_03_0034>`.

#. Click **Submit**.

   -  After you submit the scale-out application, task information of the cluster changes to **Scaling out** and the process will take several minutes. During the scale-out, the cluster automatically restarts. Therefore, the cluster status will stay **Unavailable** for a while. After the cluster is restarted, the status will change to **Available**. In the last phase of scale-out, the system dynamically redistributes user data in the cluster, during which the cluster is in the **Read-only** state.
   -  A cluster is successfully scaled out only when the cluster is in the **Available** state and task information **Scaling out** is not displayed. Then you can use the cluster.
   -  If **Scale-out failed** is displayed, the cluster fails to be scaled out.
