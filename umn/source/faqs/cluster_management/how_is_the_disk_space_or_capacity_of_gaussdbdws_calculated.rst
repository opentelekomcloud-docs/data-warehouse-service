:original_name: dws_03_0090.html

.. _dws_03_0090:

How Is the Disk Space or Capacity of GaussDB(DWS) Calculated?
=============================================================

#. Total disk capacity of GaussDB(DWS): For a three-node cluster, if each node is 320 GB, the total capacity is 960 GB. When 1 GB data is stored, GaussDB(DWS) stores 1 GB data on two nodes due to duplication, a security mechanism, thereby occupying a total of 2 GB space. As a result, more than 2 GB space is occupied if metadata and indexes are calculated. Therefore, a three-node cluster with a total capacity of 960 GB can store 480 GB data. This mechanism ensures data security.

   When you create nodes on the console, you are billed by the available capacity of a node. For example, the actual space of **dws.m3.xlarge** is 320 GB and the available space displayed is 160 GB, the space you will be billed for.

#. Check the disk usage of a single node.

   Similarly, if the total capacity is 960 GB and there are three data nodes, the disk capacity of each node is 320 GB.

   Log in to the Gauss(DWS) console and choose **Monitoring** > **Node Monitoring** > **Overview** to view the usage of disks and other resources on each node.

   .. note::

      -  The disk space displayed on the **Node Management** page is the total capacity of all disks, that is, system disks and data disks, in the data warehouse cluster. The disk space displayed on the **Overview** page is only the available space for storing table data in the cluster. In addition, tables in the data warehouse cluster have backup copies, these copies also occupy the disk storage.
      -  If the cluster is read-only and an alarm for insufficient disk space is generated, expand the cluster capacity by following the instructions provided in Scaling Out a Cluster.
