:original_name: dws_01_00082.html

.. _dws_01_00082:

Creating a DR Task
==================

Creating an Intra-Region Cluster-Level DR Task
----------------------------------------------

**Prerequisites**

You can create a DR task only when the cluster is in the **Available** or **Unbalanced** state.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. On the displayed page, click **Create**.

#. Select the type and enter the name of the DR task to be created.

   -  **Type**: **Intra-region DR**
   -  **Name**: Enter 4 to 64 case-insensitive characters, starting with a letter. Only letters, digits, hyphens (-), and underscores (_) are allowed.

#. Configure the production cluster.

   -  Select a created production cluster from the drop-down list.
   -  After a production cluster is selected, the system automatically displays its AZ.

#. Configure the DR cluster.

   -  Select the AZ associated with the region where the DR cluster resides.

      .. note::

         The AZ of the DR cluster can be the same as that of the production cluster. In a 3-AZ cluster, any of the three AZs can be selected for DR.

   -  After you select an AZ for the DR cluster, homogeneous DR clusters will be displayed. If no DR cluster is available, create a cluster with the same configurations as the production cluster.

      |image1|

#. Configure advanced parameters. Select **Default** to keep the default values of the advanced parameters. You can also select **Custom** to modify the values.

   -  The DR synchronization period indicates the interval for synchronizing incremental data from the production cluster to the DR cluster. Set this parameter based on the actual service data volume.

      .. note::

         The default DR synchronization period is 30 minutes.

#. Click **OK**.

   The DR status will then change to **Creating**. Wait until the creation is complete, and the DR status will change to **Not Started**.

.. |image1| image:: /_static/images/en-us_image_0000001711439624.png
