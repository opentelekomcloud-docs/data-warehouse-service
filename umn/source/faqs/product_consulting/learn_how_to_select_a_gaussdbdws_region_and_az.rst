:original_name: dws_03_0052.html

.. _dws_03_0052:

Learn How to Select a GaussDB(DWS) Region and AZ
================================================

Concepts
--------

A region and availability zone (AZ) identify the location of a data center. You can create resources in regions and AZs.

-  A region is a physical data center location. Each region is completely isolated to ensure high fault tolerance and stability. After creating resources in a region, you cannot change the region.
-  An AZ is a physical location with independent power supplies and network in a region. A region contains one or more AZs that are physically isolated but interconnected through high-speed internal connections. Faults that occur in one AZ will not affect other AZs. The inter-AZ connections are low-latency and unexpensive.

Selecting a Region
------------------

You are advised to select a region close to you or your target users. This reduces network latency and improves speed.

How Do I Select an AZ?
----------------------

Consider your requirements for DR and network latency when selecting an AZ:

-  Deploy resources in different AZs in the same region to improve DR.
-  For an application that requires an extremely low latency, deploy all its resources in the same AZ.

Regions and Endpoints
---------------------

When you use resources with API calls, you must specify the regional endpoint. For details about public cloud regions and endpoints, see "Regions and Endpoints".
