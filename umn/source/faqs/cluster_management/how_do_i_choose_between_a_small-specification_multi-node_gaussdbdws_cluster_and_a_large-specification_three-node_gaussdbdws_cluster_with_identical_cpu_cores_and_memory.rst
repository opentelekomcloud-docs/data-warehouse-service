:original_name: dws_03_3416.html

.. _dws_03_3416:

How Do I Choose Between a Small-Specification Multi-Node GaussDB(DWS) Cluster and a Large-Specification Three-Node GaussDB(DWS) Cluster with Identical CPU Cores and Memory?
============================================================================================================================================================================

-  Small-flavor many-node:

   If your data volume is small and you have requirement for node scaling, but you have limited budget, you can select a small-flavor many-node cluster.

   For example, a small-flavor cluster (dwsx2.h.2xlarge.4.c6) with 8 cores and 32 GB memory can provide strong computing capabilities. The cluster has a large number of nodes and can process high concurrent requests. In this case, you only need to ensure that the network speed between nodes is normal to avoid cluster performance limitation.

-  Large-flavor three-node:

   If you have a large amount of data to be processed, have high requirement on computing, and have a high budget, you can select a large-flavor three-node cluster.

   For example, a large-flavor cluster (dws2.m6.8xlarge.8) with 32 cores and 256 GB memory has faster CPU processing capability and larger memory, and can process data more quickly. However, the cluster has limited nodes, which may cause low performance in high-concurrency scenarios.
