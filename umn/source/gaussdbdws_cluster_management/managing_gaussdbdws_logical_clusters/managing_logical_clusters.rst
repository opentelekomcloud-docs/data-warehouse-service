:original_name: dws_01_7243.html

.. _dws_01_7243:

Managing Logical Clusters
=========================

Editing a Logical Cluster
-------------------------

**Precautions**

-  Nodes are added to or removed from a logical cluster by ring.
-  At least one ring must be reserved in a logical cluster.
-  The ring removed from the logical cluster will be added to the elastic cluster.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **Logical Clusters** and click **Edit** in the **Operation** column of the target cluster.
#. Add a node to the logical cluster by moving the selected ring from the right to the left, or remove a node from the logical cluster by moving the selected ring from the left to the right, and click **OK**.
#. When adding a node, select offline scale-out.

Restarting Logical Clusters
---------------------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane on the left, switch to the **Logical Clusters** page, locate the row that contains the logical cluster to be restarted, and click **More** > **Restart** in the **Operation** column.
#. Confirm that all information is correct and click **OK**.

Scaling Out a Logical Cluster
-----------------------------

**Prerequisites**

-  Before a scale-out, you need to enable the logical cluster mode and add a logical cluster.
-  After scaling out or scaling in a logical cluster, you need to reconfigure the backup policy for full backup. For details, see :ref:`Configuring an Automated Snapshot Policy <dws_01_0089>`.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Dedicated Clusters** > **Clusters**.

#. On the displayed **Clusters** page, choose **More** > **Scale Node** > **Scale Out**.

#. On the scale-out page, select a logical or elastic cluster.

   |image1|

.. |image1| image:: /_static/images/en-us_image_0000002167906652.png
