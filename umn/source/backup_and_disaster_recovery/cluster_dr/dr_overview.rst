:original_name: dws_01_00081.html

.. _dws_01_00081:

DR Overview
===========

Overview
--------

A homogeneous GaussDB(DWS) disaster recovery (DR) cluster is deployed in the same region. If the production cluster fails to provide read and write services due to natural disasters in the specified region or cluster internal faults, the DR cluster becomes the production cluster to ensure service continuity. The following figure shows the architecture.

|image1|

DR Features
-----------

-  Multi-form DR

   -  Intra-region DR
   -  Multiple data synchronization modes: synchronization layer based on mutual trust

-  Low TCO

   -  Heterogeneous deployment (logical homogeneity)
   -  Cluster-level DR

-  Visual console

   Automatic and one-click DR drills

Constraints and Limitations
---------------------------

-  During data synchronization, a non-fine-grained DR cluster cannot provide read or write services.
-  When the DR task is stopped or abnormal but the DR cluster is normal, the DR cluster can provide the read service. After the DR switchover is successful, the DR cluster can provide the read and write services.
-  When the DR task is created, the snapshot function of the production cluster is normal, but that of the DR cluster is disabled. Besides, snapshot restoration of both clusters is disabled.
-  Logical clusters are not supported.
-  Resource pools are not supported.
-  If cold and hot tables are used, cold data is synchronized using OBS.
-  DR does not synchronize data from external sources.
-  DR management refers to dual-cluster DR under the same tenant.
-  The DR cluster and the production cluster must be logically homogeneous and in the same type and version.
-  The production cluster and DR cluster used for intra-region DR must be in the same VPC.
-  In intra-region DR, after services are switched over from the production cluster to the DR cluster, the bound ELB is automatically switched to the new production cluster. During the switchover, the connection is interrupted for a short period of time. Do not run service statements to write data during the switchover.
-  During intra-region DR, the EIP, intranet domain name, and connection IP address of the original production cluster are not automatically switched with the cluster switchover. The EIP, domain name, or IP address used for connection in the service system need to be switched to the new cluster.

.. |image1| image:: /_static/images/en-us_image_0000001759518505.png
