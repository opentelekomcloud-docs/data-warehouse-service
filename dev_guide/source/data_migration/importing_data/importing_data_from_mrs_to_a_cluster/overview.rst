:original_name: dws_04_0066.html

.. _dws_04_0066:

Overview
========

MRS is a big data cluster running based on the open-source Hadoop ecosystem. It provides the industry's latest cutting-edge storage and analytical capabilities of massive volumes of data, satisfying your data storage and processing requirements. For details, see the *MapReduce Service User Guide*.

You can use Hive/Spark (analysis cluster of MRS) to store massive volumes of service data. Hive/Spark data files are stored on HDFS. On GaussDB(DWS), you can connect a GaussDB(DWS) cluster to an MRS cluster, read data from HDFS files, and write the data to GaussDB(DWS) when the clusters are on the same network.

.. important::

   Ensure that MRS can communicate with DWS:

   Scenario 1: If MRS and DWS are in the same region and VPC, they can communicate with each other by default.

   Scenario 2: If MRS and GaussDB(DWS) are in the same region but in different VPCs, you need to create a VPC peering connection. For details, see "VPC Peering Connection Overview" in *Virtual Private Cloud User Guide*.

   Scenario 3: If MRS and GaussDB(DWS) are not in the same region, you need to use Cloud Connect (CC) to create network connections. For details, see the user guide of the corresponding service.

   Scenario 4: If MRS is deployed on-premises, you need to use Direct Connect (DC) or Virtual Private Network (VPN) to connect the network. For details, see the User Guide of the corresponding service.

Importing Data from MRS to a GaussDB(DWS) Cluster
-------------------------------------------------

#. :ref:`Preparing Data in an MRS Cluster <dws_04_0212>`
#. (Optional) :ref:`Manually Creating a Foreign Server <dws_04_0213>`
#. :ref:`Creating a Foreign Table <dws_04_0214>`
#. :ref:`Importing Data <dws_04_0215>`
#. :ref:`Deleting Resources <dws_04_0216>`
