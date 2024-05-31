:original_name: dws_01_0055.html

.. _dws_01_0055:

MRS Data Source Usage Overview
==============================

MRS Cluster Overview
--------------------

MRS is a big data cluster running based on the open-source Hadoop ecosystem. It provides the industry's latest cutting-edge storage and analysis capabilities of massive volumes of data, satisfying your data storage and processing requirements. For details about MRS services, see the *MapReduce Service User Guide*.

You can use Hive/Spark (analysis cluster of MRS) to store massive volumes of service data. Hive/Spark data files are stored in HDFS. On GaussDB(DWS), you can connect a data warehouse cluster to MRS clusters, read data from HDFS files, and write the data to GaussDB(DWS) when the clusters are on the same network.

Operation Process
-----------------

Perform the following operations to import data from MRS to a data warehouse cluster:

#. Prerequisites

   a. Create an MRS cluster in a GaussDB(DWS) cluster. For details, see "Buying a Custom Cluster" in *MapReduce User Guide*.

   b. Create an HDFS foreign table for querying data from the MRS cluster over APIs of a foreign server.

      For details, see **Data Import > Importing Data from MRS to a Cluster** in the *Data Warehouse Service Database Development Guide*.

      .. note::

         -  Multiple MRS data sources can exist on the same network, but one GaussDB(DWS) cluster can connect to only one MRS cluster at a time.

#. In the data warehouse cluster, create an MRS data source connection according to :ref:`Creating an MRS Data Source Connection <dws_01_0059>`.
#. Import data from an MRS data source to the cluster. For details, see "Data Import > Importing Data from MRS to a Data Warehouse Cluster".
#. (Optional) When the HDFS configuration of the MRS cluster changes, update the MRS data source configuration on GaussDB(DWS). For details, see :ref:`Updating the MRS Data Source Configuration <dws_01_0156>`.
