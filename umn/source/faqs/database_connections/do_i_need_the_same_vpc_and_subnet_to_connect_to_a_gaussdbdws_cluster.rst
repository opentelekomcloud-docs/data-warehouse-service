:original_name: dws_03_2125.html

.. _dws_03_2125:

Do I Need the Same VPC and Subnet to Connect to a GaussDB(DWS) Cluster?
=======================================================================

If BI applications, client ECS, and DGC need to communicate with GaussDB(DWS), they must be in the same region and VPC as the GaussDB(DWS) cluster (different subnets are allowed).

In the same region, communication is allowed between different AZs in the same VPC. In the case of different VPCs, create a VPC peering connection to enable communication. For details, see .
