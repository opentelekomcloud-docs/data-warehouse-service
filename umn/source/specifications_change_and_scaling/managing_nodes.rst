:original_name: dws_01_0818.html

.. _dws_01_0818:

Managing Nodes
==============

Overview
--------

On the **Nodes** tab page, you can view the node list of the current cluster, add new nodes to or remove nodes from it, and view the node usage, status, flavor, and AZ. To modify an alias, click |image1| next to it.


.. figure:: /_static/images/en-us_image_0000001951848629.png
   :alt: **Figure 1** Managing Nodes

   **Figure 1** Managing Nodes

.. note::

   -  This feature is supported only in 8.1.1.200 or later cluster versions.

.. _en-us_topic_0000001952008065__section1755822564916:

Adding Nodes
------------

This function is more suited for large-scale scale-out. Nodes can be added in batches in advance without interrupting services. To add 180 nodes, add them in three batches of 60 nodes each. If any nodes fail to be added, retry adding them. Once all 180 nodes are added, use them for scaling out.

**Precautions**

-  Nodes can be added only when no other task is running on the management side.
-  The storage size of a new node must be the same as that of each of the existing nodes in the cluster.
-  An added node, typically for scaling out, is referred to as an idle node. You are advised to only add nodes when needed and promptly use them for scaling out.
-  The anti-affinity rule dictates that the number of nodes to be added at a time must be an integer multiple of the cluster ring size. For example, if the cluster ring size is 3, the number of nodes to be added must be an integer multiple of 3.
-  In the anti-affinity deployment mode, when a node is idle and fails due to power-off or other causes, it makes other nodes in its server group unavailable. In this case, you should remove and re-add the failed node.
-  The anti-affinity rule dictates that, if a node fails to be added and is rolled back, other nodes that are being added in the same server group will also be rolled back.

**Procedure**

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters** > **Dedicated Clusters**. All clusters are displayed by default.
#. Click the name of the target cluster. On the **Cluster Information** page that is displayed, choose **Nodes**.
#. Click **Add**, enter the number of nodes to be added, and click **Next: Confirm**. If there are not enough IP addresses in the original subnet, you can add idle nodes from other subnets.
#. After confirming that the information is correct, click **Submit**. The **Nodes** page is displayed. On this page, you can start adding nodes. Nodes that fail to be added are automatically rolled back and recorded in the failure list.

Removing Nodes
--------------

**Precautions**

-  Nodes can be removed only when no other task is running on the management side.
-  Only nodes whose resource status is **Idle** can be removed. Nodes that are in use cannot be removed.
-  In anti-affinity deployment, nodes are removed by cluster ring. For example, when you remove a node, other nodes in the same ring will be automatically selected and displayed.

**Procedure**

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters** > **Dedicated Clusters**. All clusters are displayed by default.
#. Click the name of the target cluster. On the **Cluster Information** page that is displayed, choose **Nodes**.
#. On the **Nodes** page, select the node to be deleted and click **Delete**.
#. Confirm the information and click **OK**. After the deletion is successful, the node is no longer displayed on the **Nodes** page.

.. |image1| image:: /_static/images/en-us_image_0000001924569556.png
