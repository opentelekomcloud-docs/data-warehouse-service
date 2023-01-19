:original_name: dws_01_0006.html

.. _dws_01_0006:

GaussDB(DWS) Database Concepts
==============================

Database
--------

A data warehouse cluster is an analysis-oriented relational database platform that supports online analysis.

OLAP
----

OLAP is a major function of data warehouse clusters. It supports complex analysis, provides decision-making support tailored to analysis results, and delivers intuitive query results.

MPP
---

On each node in the data warehouse cluster, memory computing and disk storage systems are independent from each other. With MPP, GaussDB(DWS) distributes service data to different nodes based on the database model and application characteristics. Nodes are connected through the network and collaboratively process computing tasks as a cluster and provide database services that meet service needs.

Shared-Nothing Architecture
---------------------------

The shared-nothing architecture is a distributed computing architecture. Each node is independent so that nodes do not compete for resources, which improves work efficiency.

Database Version
----------------

Each data warehouse cluster has a specific database version. You can check the version when creating a data warehouse cluster.

Database Connection
-------------------

You can use a client to connect to a GaussDB(DWS) cluster in the cloud and over the Internet.

Database User
-------------

You can add and control users who can access the database of a data warehouse cluster by assigning specific permissions to them. The database administrator generated when you create a cluster is the default database user.
