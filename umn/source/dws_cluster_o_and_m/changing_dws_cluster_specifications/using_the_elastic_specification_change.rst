:original_name: dws_01_0829.html

.. _dws_01_0829:

Using the Elastic Specification Change
======================================

Overview
--------

Heavy service traffic requires additional resources (such as CPU, memory, and disk resources) to support it. If the current cluster resources are insufficient, creating a new cluster with more resources may be necessary. However, this can be costly and time-consuming. Moreover, creating a cluster with many resources but low service volume can result in resource redundancy and high costs.

The elastic flavor change function is introduced to tackle this problem. It is ideal for scenarios where computing capabilities (CPU and memory) need to be adjusted during peak hours or when only computing capabilities need to be changed. By using elastic flavor change before peak hours, the cluster's computing capability can be quickly increased. After peak hours, the cluster configuration can be reduced to minimize costs.

You can modify the CPU and memory configurations of the VM nodes in the target cluster by utilizing the underlying ECS capabilities. The following figure illustrates this process.

-  To prevent service disruptions, it is crucial to schedule the elastic flavor change time window properly since the cluster must be stopped during the entire process.
-  Changing all nodes concurrently ensures that the process will not take longer due to the number of nodes. Typically, the entire process takes around 5 to 10 minutes.


.. figure:: /_static/images/en-us_image_0000002235494904.png
   :alt: **Figure 1** Principle of elastic flavor change

   **Figure 1** Principle of elastic flavor change

.. note::

   -  Only cluster versions 8.1.1.300 and later support elastic flavor change. For an earlier version, contact technical support to upgrade it first.

Precautions
-----------

-  Choosing a lower target flavor when decreasing a cluster's flavor can impact its performance, so it is crucial to assess the potential impact on services before proceeding with the operation.
-  Make sure to check if there are enough ECS resources and tenant CPU quotas in the current region before modifying the flavors.
-  You can change the flavors again if needed. In case the flavors of some nodes fail to change, you can resubmit the change task to execute the process.

Constraints and Limitations
---------------------------

-  Stop the VM before changing the flavor. The flavor change can only be done offline and it takes 5 to 10 minutes.

Procedure
---------

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.
#. In the row of a cluster, choose **More** > **Change Flavor** in the **Operation** column and click **Change Node Flavor**.
#. Configure the flavor. Enable automatic backup as needed.
#. Confirm the settings, select the confirmation check box, and click **Next: Confirm**.
#. Click **Submit**.
#. Return to the cluster list. The cluster status will change to **Changing node flavor**. Wait for about 5 to 10 minutes.
