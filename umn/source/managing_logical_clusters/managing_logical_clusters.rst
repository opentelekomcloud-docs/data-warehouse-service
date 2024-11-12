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
-  Logical clusters of version 8.1.3 and later support online scale-out.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **Logical Clusters** and click **Edit** in the **Operation** column of the target cluster.
#. Add a node to the logical cluster by moving the selected ring from the right to the left, or remove a node from the logical cluster by moving the selected ring from the left to the right, and click **OK**.
#. When adding a node, select online or offline scale-out as needed.

Managing Resources (in a Logical Cluster)
-----------------------------------------

**Precautions**

The original resource pool configuration is cleared when the cluster is converted from physical to logical. You have to add the resource pool again if you want to configure it after the conversion.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane, choose **Logical Cluster Management**. In the **Operation** column of a logical cluster, click **Resource Management Configurations**. On the displayed page, you can manage resources in a logical cluster. For details, see :ref:`Resource Management <dws_01_0723>`.

Restarting Logical Clusters
---------------------------

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane on the left, switch to the **Logical Clusters** page, locate the row that contains the logical cluster to be restarted, and click **More** > **Restart** in the **Operation** column.
#. Confirm that all information is correct and click **OK**.

Scaling Out a Logical Cluster
-----------------------------

**Prerequisites**

-  Logical clusters of version 8.1.3 and later support online scale-out.
-  Before a scale-out, you need to enable the logical cluster mode and add a logical cluster.
-  After scaling out or scaling in a logical cluster, you need to reconfigure the backup policy for full backup. For details, see :ref:`Configuring an Automated Snapshot Policy <dws_01_0089>`.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.

#. On the displayed **Clusters** page, choose **More** > **Scale Node** > **Scale Out**.

#. On the scale-out page, select a logical or elastic cluster.

   .. note::

      Clusters of version 8.2.1.100 and later support job termination.

   |image1|

Scaling In a Logical Cluster
----------------------------

**Constraints**

-  A host ring with CN nodes cannot be scaled in.
-  A host ring with GTM and CM nodes cannot be scaled in.

**Procedure**

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.
#. In the navigation pane on the left, switch to the **Logical Clusters** page. Locate the row that contains the target elastic pool and click **Edit** in the **Operation** column to delete nodes from the elastic pool (move the selected ring from the left to the right).
#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001952008797.png
