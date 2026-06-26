:original_name: dws_01_7242.html

.. _dws_01_7242:

Adding/Deleting a Logical Cluster
=================================

Adding a Logical Cluster
------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. Enable **Logical Cluster**. The **Logical Clusters** menu item will be displayed in the navigation pane.

   |image1|

#. Switch to the **Logical Clusters** page and click **Add Logical Cluster**. (If the current cluster has service data, you can only convert the cluster into a logical cluster.) In the displayed dialog box, click **OK** to convert the cluster to a logical cluster. Then, you can add nodes to the elastic cluster and add a new logical cluster.

   -  If you access the **Logical Clusters** page for the first time, the metadata of the logical cluster created at the backend is synchronized to the frontend. After the synchronization is complete, you can view information about the logical clusters at the frontend. The logical cluster name is case sensitive. For example, metadata of **lc1** and **LC1** cannot be synchronized.
   -  The original resource pool configuration is cleared when the cluster is converted from physical to logical. The resource pool information configured after the cluster is converted to a logical cluster will be bound to the logical cluster.

#. Move the ring you want to add from the right to the left panel, enter the logical cluster name, and click **OK**.

Deleting Logical Clusters
-------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.
#. In the navigation pane, choose **Logical Clusters**. Locate the row that contains the logical cluster to be deleted, click **More** in the **Operation** column, and select **Delete**.

   -  The first manually added logical cluster cannot be deleted.

   -  Nodes of the deleted logical cluster are added to the elastic cluster.
   -  If a read-only logical cluster is manually deleted, the status of the one-off deletion plan changes to the completed state. For a read-only logical cluster that is bound to a periodic addition or deletion plan, the next execution time in the deletion column of the plan is automatically updated to the next period or the end of the period after the plan is manually deleted.

#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000002235494760.png
