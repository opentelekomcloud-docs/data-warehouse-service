:original_name: dws_03_0028.html

.. _dws_03_0028:

How Much Service Data Can Be Stored in GaussDB(DWS)?
====================================================

Each node in a GaussDB(DWS) cluster has a default storage capacity of 160 GB, 256 GB, 1.6 TB, 1.8 TB, or 13 TB. A cluster can house 3 to 256 nodes and the total storage capacity of the cluster expands proportionally as the cluster scale grows.

To enhance reliability, each node has a copy, which occupies half of the storage space.

The GaussDB(DWS) system creates backups, indexes, temporary cache files, and run logs that take up storage space. Each node stores data that accounts for approximately half of the total storage capacity.
