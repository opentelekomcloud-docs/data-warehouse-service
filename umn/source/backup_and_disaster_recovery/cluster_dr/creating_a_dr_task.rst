:original_name: dws_01_00082.html

.. _dws_01_00082:

Creating a DR Task
==================

Creating a Cross-AZ DR Task
---------------------------

**Prerequisites**

You can create a DR task only when the cluster is in the **Available** or **Unbalanced** state.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. On the displayed page, click **Create**.

#. Select the type and enter the name of the DR task to be created.

   -  **Type**: **Cross-AZ DR**
   -  **Name**: Enter 4 to 64 case-insensitive characters, starting with a letter. Only letters, digits, hyphens (-), and underscores (_) are allowed.

#. Configure the production cluster.

   -  Select a created production cluster from the drop-down list.

   -  After a production cluster is selected, the system automatically displays its AZ.

      |image1|

#. Configure the DR cluster.

   -  Select the AZ associated with the region where the DR cluster resides.

      .. note::

         The production cluster AZ will be filtered out from the available DR cluster AZs.

   -  After you select an AZ for the DR cluster, homogeneous DR clusters will be displayed. If no DR cluster is available, create a cluster with the same configurations as the production cluster.

      |image2|

#. Configure advanced parameters. Select **Default** to keep the default values of the advanced parameters. You can also select **Custom** to modify the values.

   -  The DR synchronization period indicates the interval for synchronizing incremental data from the production cluster to the DR cluster. Set this parameter based on the actual service data volume.

      |image3|

      .. note::

         The default DR synchronization period is 30 minutes.

#. Click **OK**.

   The DR status will then change to **Creating**. Wait until the creation is complete, and the DR status will change to **Not Started**.

.. |image1| image:: /_static/images/en-us_image_0000001518034009.png
.. |image2| image:: /_static/images/en-us_image_0000001686989241.png
.. |image3| image:: /_static/images/en-us_image_0000001466914462.png
