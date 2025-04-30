:original_name: dws_03_0090.html

.. _dws_03_0090:

How Is the Disk Space or Capacity of GaussDB(DWS) Calculated?
=============================================================

#. Total disk capacity of a GaussDB(DWS) cluster: Taking three data nodes in the cluster as an example, assume each node has a capacity of 320 GB, resulting in a total capacity of 960 GB. When 1 GB of data is stored, GaussDB(DWS) employs its replication mechanism to maintain copies of the data across two nodes, thereby utilizing 2 GB of storage space. With additional metadata and indexing considerations, although the original dataset remains at 1 GB, the actual storage requirements exceed 2 GB upon ingestion into GaussDB(DWS). Therefore, a three-node cluster with a total capacity of 960 GB can store 480 GB data. This mechanism ensures data security.

   When you create a cluster on the GaussDB(DWS) console, the actual capacity of a node is displayed on the page. For example, the disk size of the **dwsx2.xlarge** node is 160 GB on the creation page. However, the actual disk size of the node is 320 GB, which is displayed as 160 GB. In this way, you can create a node based on the actual disk data.

#. Check the disk usage of a single node.

   Similarly, if the total capacity is 960 GB and there are three data nodes, the disk capacity of each node is 320 GB.

   Log in to the GaussDB(DWS) console and choose **Monitoring** > **Node Monitoring** > **Overview** to view the usage of disks and other resources on each node.

   .. note::

      -  The disk space shown on the node management page represents the combined capacity of all disks in the GaussDB(DWS) cluster, including system disks and data disks. On the overview page, the displayed disk space only refers to the available space for storing table data in the cluster. Additionally, the GaussDB(DWS) cluster has backup copies of tables, which also occupy disk storage.
      -  If the cluster is read-only and an alarm for insufficient disk space is generated, scale out the cluster by following the instructions provided in "Scaling Out a Cluster".
