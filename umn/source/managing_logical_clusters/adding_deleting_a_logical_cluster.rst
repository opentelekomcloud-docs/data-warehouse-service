:original_name: dws_01_7242.html

.. _dws_01_7242:

Adding/Deleting a Logical Cluster
=================================

Adding a Logical Cluster
------------------------

**Precautions**

-  If you access the **Logical Clusters** page for the first time, the metadata of the logical cluster created at the backend is synchronized to the frontend. After the synchronization is complete, you can view information about the logical clusters at the frontend. The logical cluster name is case sensitive. For example, metadata of **lc1** and **LC1** cannot be synchronized.
-  The original resource pool configuration is cleared when the cluster is converted from physical to logical. The resource pool information configured after the cluster is converted to a logical cluster will be bound to the logical cluster.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. Enable **Logical Clusters**. The **Logical Clusters** menu item will be displayed in the navigation pane on the left.

   |image1|

#. Go to the **Logical Clusters** tab and click **Add Logical Cluster**.

#. Move the ring you want to add from the right to the left panel, enter the logical cluster name, and click **OK**.

Deleting Logical Clusters
-------------------------

**Precautions**

-  The first manually added logical cluster cannot be deleted.

-  Nodes of the deleted logical cluster are added to the elastic cluster.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane on the left, switch to the **Logical Clusters** page, locate the row that contains the logical cluster to be deleted, and click **Delete** in the **Operation** column.
#. Confirm that all information is correct and click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001952008513.png
