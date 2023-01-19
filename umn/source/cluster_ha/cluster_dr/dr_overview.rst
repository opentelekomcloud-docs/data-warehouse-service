:original_name: dws_01_00081.html

.. _dws_01_00081:

DR Overview
===========

Overview
--------

A homogeneous GaussDB(DWS) DR cluster is deployed in another AZ. If the production cluster fails to provide read and write services due to natural disasters in the specified region or cluster internal faults, the DR cluster becomes the production cluster to ensure service continuity. The following figure shows the architecture.

|image1|

.. note::

   -  This feature is supported only in cluster version 8.1.1 or later.

DR Features
-----------

-  Multi-form DR

   -  Cross-AZ DR
   -  Multiple data synchronization modes: synchronization layer based on mutual trust

-  Low TCO

   -  Heterogeneous deployment (logical homogeneity)
   -  Cluster-level DR

-  Visual console

   -  Automatic and one-click DR drills

Constraints and Limitations
---------------------------

-  During data synchronization, the DR cluster cannot provide read or write services.
-  When the DR task is stopped or abnormal but the DR cluster is normal, the DR cluster can provide the read service. After the DR switchover is successful, the DR cluster can provide the read and write services.
-  When the DR task is created, the snapshot function of the production cluster is normal, but that of the DR cluster is disabled. Besides, snapshot restoration of both clusters is disabled.
-  Logical clusters are not supported.
-  DR refers to dual-cluster DR of the same tenant.
-  The production cluster and DR cluster are in the same VPC and of the same version.

.. |image1| image:: /_static/images/en-us_image_0000001172481700.png
