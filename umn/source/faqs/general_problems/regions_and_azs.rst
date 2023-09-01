:original_name: dws_03_0052.html

.. _dws_03_0052:

Regions and AZs
===============

Concepts
--------

A region and availability zone (AZ) identify the location of a data center. You can create resources in regions and AZs.

-  A region is a physical data center. Each region is completely isolated to ensure high fault tolerance and stability. After creating resources in a region, you cannot change the region.
-  An AZ is a physical location with independent power supplies and network in a region. A region contains one or more AZs that are physically isolated but interconnected through the internal network. The fault of an AZ will not affect other AZs. The internal network provides economical connection with low latency.

:ref:`Figure 1 <en-us_topic_0000001381728553__fig8305184814287>` shows the relationship between the regions and AZs.

.. _en-us_topic_0000001381728553__fig8305184814287:

.. figure:: /_static/images/en-us_image_0000001636951157.png
   :alt: **Figure 1** Regions and AZs

   **Figure 1** Regions and AZs

Selecting a Region
------------------

You are advised to select a region close to you or your target users. This reduces network latency and improves access rate.

How Do I Select an AZ?
----------------------

Consider your requirements for DR and network latency when selecting an AZ:

-  Deploy resources in different AZs in the same region for DR purposes.
-  Deploy resources in the same AZ for minimum latency.

Regions and Endpoints
---------------------

When you use resources with API calls, you must specify the regional endpoint. For details about public cloud regions and endpoints, see "Regions and Endpoints".
