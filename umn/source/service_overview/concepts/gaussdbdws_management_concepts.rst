:original_name: dws_01_0005.html

.. _dws_01_0005:

GaussDB(DWS) Management Concepts
================================

Cluster
-------

A cluster is a server group that consists of multiple nodes. GaussDB(DWS) is organized using clusters. A data warehouse cluster contains nodes with the same flavor in the same subnet. These nodes work together to provide services.

Node
----

Each data warehouse cluster contains at least three nodes, all of which support data storage and analysis.

Flavor
------

You need to specify the node flavors when you create a data warehouse cluster. CPU, memory, and storage resources vary depending on node flavors.

Snapshot
--------

You can create snapshots to back up GaussDB(DWS) cluster data. A snapshot is retained until you delete it on the management console. Automated snapshots cannot be manually deleted. Snapshots will occupy your OBS quotas.

Project
-------

Projects are used to group and isolate OpenStack resources (computing resources, storage resources, and network resources). A project can be a department or a project team. Multiple projects can be created for one account.
