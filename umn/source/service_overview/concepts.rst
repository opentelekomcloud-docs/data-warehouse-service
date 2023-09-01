:original_name: dws_01_0004.html

.. _dws_01_0004:

Concepts
========

GaussDB(DWS) Management Concepts
--------------------------------

-  Cluster

   A cluster is a server group that consists of multiple nodes. GaussDB(DWS) is organized using clusters. A data warehouse cluster contains nodes with the same flavor in the same subnet. These nodes work together to provide services.

-  Node

   A GaussDB(DWS) cluster can have 3 to 256 nodes. A hybrid data warehouse (standalone) can only have one node. Each node can store and analyze data.

-  Type

   You need to specify the node flavors when you create a data warehouse cluster. CPU, memory, and storage resources vary depending on node flavors.

-  Snapshot

   You can create snapshots to back up GaussDB(DWS) cluster data. A snapshot is retained until you delete it on the management console. Automated snapshots cannot be manually deleted. Snapshots will occupy your OBS quotas.

-  Project

   Projects are used to group and isolate OpenStack resources (computing resources, storage resources, and network resources). A project can be a department or a project team. Multiple projects can be created for one account.

GaussDB(DWS) Database Concepts
------------------------------

-  Databases

   A data warehouse cluster is an analysis-oriented relational database platform that supports online analysis.

-  OLAP

   OLAP is a major function of data warehouse clusters. It supports complex analysis, provides decision-making support tailored to analysis results, and delivers intuitive query results.

-  MPP

   On each node in the data warehouse cluster, memory computing and disk storage systems are independent from each other. With MPP, GaussDB(DWS) distributes service data to different nodes based on the database model and application characteristics. Nodes are connected through the network and collaboratively process computing tasks as a cluster and provide database services that meet service needs.

-  Shared-Nothing Architecture

   The shared-nothing architecture is a distributed computing architecture. Each node is independent so that nodes do not compete for resources, which improves work efficiency.

-  Database Version

   Each data warehouse cluster has a specific database version. You can check the version when creating a data warehouse cluster.

-  Database Connections

   You can use a client to connect to the GaussDB(DWS) cluster. The client can be used for connection on the cloud platform and over the Internet.

-  Database User

   You can add and control users who can access the database of a data warehouse cluster by assigning specific permissions to them. The database administrator generated when you create a cluster is the default database user.
