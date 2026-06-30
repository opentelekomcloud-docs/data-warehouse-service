:original_name: dws_01_7243.html

.. _dws_01_7243:

Managing Logical Clusters
=========================

Binding Users to Logical Clusters
---------------------------------

Assume that logical clusters **lc1** and **lc2** are available. Use the following syntax to bind **new** and **existing** users to the logical clusters.

-  Connect to the DWS database and run the following statements to bind new users **u1** and **u2** to logical clusters **lc1** and **lc2**, respectively:

   ::

      CREATE USER u1 NODE GROUP "lc1" PASSWORD '{password}';
      CREATE USER u2 NODE GROUP "lc2" PASSWORD '{password}';

-  Connect to the DWS database and run the following statement to bind existing user **u1** to logical cluster **lc1**:

   ::

      ALTER USER u1 NODE GROUP "lc1";

Run the following statement to query the logical cluster bound to a user:

::

   SELECT NODEGROUP FROM PG_USER WHERE usename = 'u1';

.. _en-us_topic_0000002235334768__section195538591424:

Editing a Logical Cluster
-------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.
#. In the navigation pane, choose **Logical Clusters**. In the **Operation** column of a logical cluster, click **Edit**.
#. Add a node to the logical cluster by moving the selected ring from the right to the left, or remove a node from the logical cluster by moving the selected ring from the left to the right, and click **OK**.

   -  Nodes are added to or removed from a logical cluster by ring.
   -  At least one ring must be reserved in a logical cluster.
   -  The ring removed from the logical cluster will be added to the elastic cluster.

#. When adding a node, select as needed.
#. Click **OK**.

Managing Resources (in a Logical Cluster)
-----------------------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Logical Clusters**. In the **Operation** column of a logical cluster, click **Resource Management**. On the displayed page, you can manage resources in the logical cluster. For details, see :ref:`DWS Resource Load Management <dws_01_0723>`.

   When a physical cluster is converted to a logical cluster, the original resource pool configuration is cleared. You have to **add the resource pool again** if you want to configure it after the conversion.

Restarting a Logical Cluster
----------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.
#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.
#. In the navigation pane, choose **Logical Clusters**. Locate the row that contains the logical cluster to be restarted, click **More** in the **Operation** column, and select **Restart**.
#. Click **OK**.

Scaling Out a Logical Cluster
-----------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, locate the target cluster, click **More** in the **Operation** column, and choose **Scale Node** > **Scale Out**. When :ref:`editing a logical cluster <en-us_topic_0000002235334768__section195538591424>`, select the offline scale-out option.

   Before scaling out the cluster, it is crucial to verify if it meets the inspection conditions. Click **Immediate Inspection** to complete the inspection and proceed to the next step only if it passes. For more information, see :ref:`Viewing Inspection Results <dws_01_01755>`.

#. On the scale-out page, select a logical or elastic cluster.

   -  Before a scale-out, you need to enable the logical cluster mode and add a logical cluster.

#. Confirm the settings, select **I agree**, and click **Next: Confirm**. On the displayed page, you can view the node specifications, scale-out mode, and the number of nodes before and after the scale-out.

#. Confirm all configurations and click **Submit**. In the displayed dialog box, click **OK** to start the scale-out.

Querying the Logical Cluster List
---------------------------------

You can query the logical cluster list on the **Logical Clusters** page of the DWS console or from the PGXC_GROUP system catalog.

Connect to the DWS database and run the following statement:

::

   SELECT * FROM PGXC_GROUP;
