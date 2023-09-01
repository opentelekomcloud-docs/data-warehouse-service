:original_name: dws_01_00081.html

.. _dws_01_00081:

DR Overview
===========

Overview
--------

A homogeneous GaussDB(DWS) disaster recovery (DR) cluster is deployed in another AZ. If the production cluster fails to provide read and write services due to natural disasters in the specified region or cluster internal faults, the DR cluster becomes the production cluster to ensure service continuity. The following figure shows the architecture.

|image1|

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
-  Resource pools are not supported.
-  If cold and hot tables are used, cold data is synchronized using OBS.
-  DR does not synchronize data from external sources.
-  DR management refers to dual-cluster DR under the same tenant.
-  The DR cluster and the production cluster must be logically homogeneous and in the same type and version.
-  The production cluster and DR cluster used for cross-AZ DR must be in the same VPC.
-  In cross-AZ DR, the DR cluster can be automatically bound to the EIP of the production cluster after a switchover.

.. |image1| image:: /_static/images/en-us_image_0000001517913905.png
