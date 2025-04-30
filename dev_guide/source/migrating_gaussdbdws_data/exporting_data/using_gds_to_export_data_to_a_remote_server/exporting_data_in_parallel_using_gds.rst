:original_name: dws_04_0262.html

.. _dws_04_0262:

Exporting Data In Parallel Using GDS
====================================

In high-concurrency scenarios, you can use GDS to export data from a database to a common file system.

In the current GDS version, data can be exported from a database to a pipe file.

-  When the local disk space of the GDS user is insufficient:

   -  The data exported from GDS is compressed using the pipe to occupy less disk space.
   -  The exported data is transferred through the pipe to the HDFS server for storage.

-  If you need to cleanse data before exporting data:

   -  You can compile programs as needed and read streaming data from pipes in real time.

   .. note::

      -  The current version does not support data export through GDS in SSL mode. Do not use GDS in SSL mode.
      -  All pipe files mentioned in this section refer to named pipes on Linux.

-  To ensure the correctness of data import or export using GDS, you need to import or export data in the same compatibility mode.

   If data is imported or exported in MySQL compatibility mode, it can only be exported or imported in the same mode.

Overview
--------

**Using foreign tables**: A GDS foreign table specifies the exported file format and export mode. Data is exported in parallel through multiple DNs from the database to data files, which improves the overall data export performance. The data files cannot be directly exported to HDFS.

-  The CN only plans data export tasks and delivers the tasks to DNs. In this case, the CN is released to process other tasks.

-  In this way, the computing capabilities and bandwidths of all the DNs are fully leveraged to export data.


   .. figure:: /_static/images/en-us_image_0000001188163824.png
      :alt: **Figure 1** Exporting data using foreign tables

      **Figure 1** Exporting data using foreign tables

Related Concepts
----------------

-  **Data file**: A TEXT, CSV, or FIXED file that stores data exported from the GaussDB(DWS) database.
-  **Foreign table**: A table that stores information, such as the format, location, and encoding format of a data file.
-  **GDS**: A data service tool. To export data, deploy it on the server where data files are stored.
-  **Table**: Tables in the database, including row-store tables and column-store tables. Data in the data files is exported from these tables.
-  **Remote mode**: Service data in a cluster is exported to hosts outside the cluster.

Exporting a Schema
------------------

Data can be exported to GaussDB(DWS) in **Remote** mode.

-  **Remote mode**: Service data in a cluster is exported to hosts outside the cluster.

   -  In this mode, multiple GDSs are used to concurrently export data. One GDS can export data for only one cluster at a time.
   -  The data export rate of a GDS that resides on the same intranet as cluster nodes is limited by the network bandwidth. A 10GE configuration is recommended.
   -  Data files in TEXT, FIXED, or CSV format are supported. The size of data in a single row must be less than 1 GB.

Data Export Process
-------------------


.. figure:: /_static/images/en-us_image_0000001233563377.png
   :alt: **Figure 2** Concurrent data export

   **Figure 2** Concurrent data export

.. table:: **Table 1** Process description

   +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Process                 | Description                                                                                                                                                                                      | Subtask               |
   +=========================+==================================================================================================================================================================================================+=======================+
   | Plan data export.       | Prepare data to be exported and plan the export path for the mode to be selected.                                                                                                                | ``-``                 |
   |                         |                                                                                                                                                                                                  |                       |
   |                         | For details, see :ref:`Planning Data Export <en-us_topic_0000001811609749>`.                                                                                                                     |                       |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Start GDS.              | If the **Remote** mode is selected, install, configure, and start GDS on data servers.                                                                                                           | ``-``                 |
   |                         |                                                                                                                                                                                                  |                       |
   |                         | For details, see :ref:`Installing, Configuring, and Starting GDS <en-us_topic_0000001811491133>`.                                                                                                |                       |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Create a foreign table, | Create a foreign table to help GDS specify information about a data file. The foreign table stores information, such as the location, format, encoding, and inter-data delimiter of a data file. | ``-``                 |
   |                         |                                                                                                                                                                                                  |                       |
   |                         | For details, see :ref:`Creating a GDS Foreign Table <en-us_topic_0000001764650532>`.                                                                                                             |                       |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Export data.            | After the foreign table is created, run the **INSERT** statement to efficiently export data to data files.                                                                                       | ``-``                 |
   |                         |                                                                                                                                                                                                  |                       |
   |                         | For details, see :ref:`Exporting Data <en-us_topic_0000001764650824>`.                                                                                                                           |                       |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Stop GDS.               | Stop GDS after data is exported.                                                                                                                                                                 | ``-``                 |
   |                         |                                                                                                                                                                                                  |                       |
   |                         | For details, see :ref:`Stopping GDS <en-us_topic_0000001764650296>`.                                                                                                                             |                       |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
