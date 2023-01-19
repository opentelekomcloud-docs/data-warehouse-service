:original_name: dws_04_0195.html

.. _dws_04_0195:

Importing Data
==============

This section describes how to create tables in GaussDB(DWS) and import data to the tables.

Before importing all the data from a table containing over 10 million records, you are advised to import some of the data, and check whether there is data skew and whether the distribution keys need to be changed. Troubleshoot the data skew if any. It is costly to address data skew and change the distribution keys after a large amount of data has been imported.

Prerequisites
-------------

The GDS server can communicate with GaussDB(DWS).

-  You need to create an ECS as the GDS server.
-  The created ECS and GaussDB(DWS) must belong to the same region, VPC, and subnet.

Procedure
---------

#. Create a table in GaussDB(DWS) to store imported data. For details, see CREATE TABLE.

#. Import data.

   ::

      INSERT INTO [Target table name] SELECT * FROM [Foreign table name]

   -  If information similar to the following is displayed, the data has been imported. Query the error information table to check whether any data format errors occurred. For details, see :ref:`Handling Import Errors <dws_04_0196>`.

      .. code-block::

         INSERT 0 9

   -  If data fails to be loaded, troubleshoot the problem by following the instructions provided in :ref:`Handling Import Errors <dws_04_0196>` and try again.

   .. note::

      -  If a data loading error occurs, the entire data import task will fail.
      -  Compile a batch-processing task script to concurrently import data. The degree of parallelism (DOP) depends on the server resource usage. You can test-import several tables, monitor resource utilization, and increase or reduce concurrency accordingly. Common resource monitoring commands include **top** for monitoring memory and CPU usage, **iostat** for monitoring I/O usage, and **sar** for monitoring networks. For details about application cases, see .
      -  If possible, more GDS servers can significantly improve the data import efficiency. For details about application cases, see :ref:`Importing Data in Parallel from Multiple Data Servers <en-us_topic_0000001145494625__section2041618243291>`.
      -  In a scenario where many GDS servers import data concurrently, you can increase the TCP Keepalive interval for connections between GDS servers and DNs to ensure connection stability. (The recommended interval is 5 minutes.) TCP Keepalive settings of the cluster affect its fault detection response time.

Example:
--------

#. Create a target table named **reasons**.

   ::

      CREATE TABLE reasons
      (
        r_reason_sk   integer  not null,
        r_reason_id   char(16) not null,
        r_reason_desc char(100)
      )
      DISTRIBUTE BY HASH (r_reason_sk);

#. Import data from source data files through the **foreign_tpcds_reasons** foreign table to the **reasons** table.

   ::

      INSERT INTO reasons SELECT * FROM foreign_tpcds_reasons ;

#. You can create indexes again after the import is complete.

   ::

      CREATE INDEX reasons_idx ON reasons(r_reasons_id);
