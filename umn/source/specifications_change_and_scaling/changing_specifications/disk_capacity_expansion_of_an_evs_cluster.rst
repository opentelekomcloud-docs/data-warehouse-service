:original_name: dws_01_0828.html

.. _dws_01_0828:

Disk Capacity Expansion of an EVS Cluster
=========================================

Context
-------

In conventional scaling, compute and storage resources are coupled. If a company scales out disks, it has to add unnecessary CPUs and memory at the same time. The scaling takes a long time and interrupts services. Disk capacity expansion can quickly increase storage without service interruption. You can increase disk space without having to stop services.

.. note::

   -  Disk capacity can be expanded only if the cluster is in **Available**, **To be restarted**, **Read-only**, or **Node fault**, **Unbalanced** state.

Precautions
-----------

-  Hot storage disks cannot be scaled down.
-  The disk capacity of each node can be expanded to up to 2000 GB.
-  A cluster can have up to 5000 disks. After scaling, the node storage capacity is evenly distributed based on the number of disks.
-  If the hot data storage capacity is increased and exceeds the quota in the discount package, excess capacity will be billed per use.
-  Scale up hot data storage during off-peak hours.
-  If the cluster is in the read-only state, a message will be displayed after you click **Expand Disk Capacity**. After you start expansion, wait until it is completed and the cluster changes to the available state.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Change Specifications** and click **Change disk capacity**. The **Change disk capacity** page is displayed.

   |image1|

#. Set the capacity and click **Resize Cluster Now**.

   |image2|

#. Confirm the settings and click **Submit**.

#. Return to the cluster list and check the disk capacity expansion progress.

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000001517355457.png
.. |image2| image:: /_static/images/en-us_image_0000001507057774.png
.. |image3| image:: /_static/images/en-us_image_0000001466914414.png
