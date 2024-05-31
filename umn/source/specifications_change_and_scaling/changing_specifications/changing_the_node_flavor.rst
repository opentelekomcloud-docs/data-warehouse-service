:original_name: dws_01_0829.html

.. _dws_01_0829:

Changing the Node Flavor
========================

If you only need to cope with occasional service peaks or only increase computing capabilities, you are advised to modify cluster specifications instead of adding nodes. Before a service peak, you can modify cluster specifications to quickly improve computing capabilities. After the service peak, you can quickly reduce cluster configurations to minimize costs.

.. note::

   -  Only cluster versions 8.1.1.300 and later support elastic flavor change. For an earlier version, contact technical support to upgrade it first.
   -  Currently, specifications can be modified only for offline clusters. The modification takes about 10 minutes.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters** > **Dedicated Clusters**. All clusters are displayed by default.

#. In the row of a cluster, choose **More** > **Change Specifications** in the **Operation** column and click **Change node flavor**.

   |image1|

#. Configure the flavor. Enable automatic backup as needed.

   |image2|

   .. important::

      Decreasing the specifications of a cluster is to select the target specifications that are lower than the current specifications of the cluster. This operation may affect the cluster performance. Therefore, evaluate service impact before performing this operation.

#. Click **Next: confirm**.

#. Return to the cluster list. The cluster status will change to **Changing node flavor**. Wait for about 10 minutes.

.. |image1| image:: /_static/images/en-us_image_0000001758887437.png
.. |image2| image:: /_static/images/en-us_image_0000001710968040.png
