:original_name: dws_01_0023.html

.. _dws_01_0023:

Scaling Out a Cluster
=====================

When you need more compute and storage resources, add more nodes for cluster scale-out on the management console.

After the data in a data warehouse is deleted, the occupied disk space may not be released, resulting in dirty data and disk waste. Therefore, if you need to scale out your cluster due to insufficient storage capacity, run the **VACUUM** command to reclaim the storage space first. If the used storage capacity is still high after you run the **VACUUM** command, you can scale out your cluster. For details about the **VACUUM** syntax, see "SQL References > SQL Syntax > VACUUM" in the *Data Warehouse Service (DWS) Developer Guide*.

Impact on the System
--------------------

-  Before the scale-out, disable the client connections that have created temporary tables because temporary tables created before or during the scale-out will become invalid and operations performed on these temporary tables will fail. Temporary tables created after the scale-out will not be affected.
-  During the scale-out, functions such as cluster restart, scale-out, snapshot creation, database administrator password resetting, and cluster deletion are disabled.
-  During an offline scale-out, the cluster automatically restarts. Therefore, the cluster stays **Unavailable** for a period of time. After the cluster is restarted, the status becomes **Available**. After scale-out, the system dynamically redistributes user data among all nodes in the cluster.

Prerequisites
-------------

-  The cluster to be scaled out is in the **Available** or **Unbalanced** state.
-  The number of nodes to be added must be less than or equal to the available nodes. Otherwise, system scale-out is not allowed.


Scaling Out a Cluster
---------------------

.. note::

   -  A cluster becomes read-only during scale-out. Exercise caution when performing this operation.
   -  To ensure data security, you are advised to create a snapshot before the scale-out. For details about how to create a snapshot, see :ref:`Manual Snapshots <dws_01_0092>`.

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters** > **Dedicated Clusters**.

   All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Scale Out**. The scale-out page is displayed.

#. Specify the number of nodes to be added.

   -  The number of nodes after scale-out must be at least three nodes more than the original number. The maximum number of nodes that can be added depends on the available quota. In addition, the number of nodes after the scale-out cannot exceed 32.

      If the node quota is insufficient, click **Increase quota** to submit a service ticket and apply for higher node quota.

   -  Flavor of the new nodes must be the same as that of existing nodes in the cluster.

   -  The VPC, subnet, and security group of the cluster with new nodes added are the same as those of the original cluster.

#. .. _en-us_topic_0000001707254605__en-us_topic_0000001372839430_li85162515474:

   Configure advanced parameters.

   -  If you choose **Default**, **Scale Online** will be disabled, **Auto Redistribution** will be enabled, and **Redistribution Mode** will be **Offline** by default.
   -  If you choose **Custom**, you can configure the following advanced configuration parameters for online scale-out:

      -  **Scale Online**: Online scale-out can be enabled. During online scale-out, data can be added, deleted, modified, and queried in the database; and some DDL syntaxes are supported. Errors will be reported for unsupported syntaxes.
      -  **Auto Redistribution**: Automatic redistribution can be enabled. If automatic redistribution is enabled, data will be redistributed immediately after the scale-out is complete. If this function is disabled, only the scale-out is performed. In this case, to redistribute data, select a cluster and choose **More** > **Scale Node** > **Redistribute**.
      -  **Redistribution Concurrency**: If automatic redistribution is enabled, you can set the number of concurrent redistribution tasks. The value range is 1 to 32. The default value is 4.
      -  **Redistribution Mode**: It can be set to **Online** or **Offline**. After confirming that the information is correct, click **OK** in the displayed dialog box.

#. Click **Next: Confirm**.

#. Click **Submit**.

   -  After you submit the scale-out application, task information of the cluster changes to **Scaling out** and the process will take several minutes. During the scale-out, the cluster automatically restarts. Therefore, the cluster status will stay **Unavailable** for a while. After the cluster is restarted, the status will change to **Available**. In the last phase of scale-out, the system dynamically redistributes user data in the cluster, during which the cluster is in the **Read-only** state.
   -  A cluster is successfully scaled out only when the cluster is in the **Available** state and task information **Scaling out** is not displayed. Then you can use the cluster.
   -  If **Scale-out failed** is displayed, the cluster fails to be scaled out.

Viewing Scaling Details
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters** > **Dedicated Clusters**.

#. In the **Task Information** column of a cluster, click **View Details**.

#. Check the scale-out status of the cluster on the scaling details page.

   |image1|

.. |image1| image:: /_static/images/en-us_image_0000001758846097.png
