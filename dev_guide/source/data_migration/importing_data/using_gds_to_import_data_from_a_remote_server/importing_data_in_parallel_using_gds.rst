:original_name: dws_04_0190.html

.. _dws_04_0190:

Importing Data In Parallel Using GDS
====================================

**INSERT** and **COPY** statements can be used only for serially importing a small volume of data. To import a large volume of data to GaussDB(DWS), you can use GDS to import data in parallel using a foreign table.

In the current GDS version, you can import data to databases from pipe files.

-  When the local disk space of the GDS user is insufficient, HDFS data can be directly written to the pipe file without occupying extra disk space.
-  To clean data before importing, you can compile a program as needed and write the data to be processed into a pipe file.

   .. note::

      -  The current version does not support data import through GDS in SSL mode. Do not use GDS in SSL mode.

      -  All pipe files mentioned in this section refer to named pipes on Linux.

      -  To ensure the correctness of data import or export using GDS, you need to import or export data in the same compatibility mode.

         If data is imported or exported in MySQL compatibility mode, it can only be exported or imported in the same mode.

Overview
--------

You can import data in parallel from the common file system (excluding HDFS) of a server to GaussDB(DWS).

Data files to be imported are specified based on the import policy and data formats set in a foreign table. Data is imported in parallel through multiple DNs from source data files to the database, which improves the overall data import performance. :ref:`Figure 1 <en-us_topic_0000001717256732__en-us_topic_0000001233883381_fc07608abaf6e4b509fbbfe774a8a548e>` shows an example.

-  The CN only plans data import tasks and delivers the tasks to DNs. In this case, the CN is released to process other tasks.
-  In this way, the computing capacities and bandwidths of all the DNs are fully leveraged to import data, improving the import performance.

You can pre-process data (such as invalid character replacement and fault tolerance processing) by setting parameters in a foreign table.

.. _en-us_topic_0000001717256732__en-us_topic_0000001233883381_fc07608abaf6e4b509fbbfe774a8a548e:

.. figure:: /_static/images/en-us_image_0000001799299941.png
   :alt: **Figure 1** Importing data in parallel

   **Figure 1** Importing data in parallel

The concepts mentioned in the preceding figure are described as follows:

-  **CN**: coordinator of GaussDB(DWS). After receiving import SQL requests from an application or client, the CN plans import tasks and delivers the tasks to DNs.
-  **DN (Datanode)**: data node of GaussDB(DWS). After receiving the import tasks delivered by the CN, DNs import data from the source data file to the target table in the database through a foreign table.
-  **Source data file**: a file that stores data to be imported.
-  **Data server**: a server that stores source data files. For security purposes, it is recommended that the data server and GaussDB(DWS) be on the same intranet.
-  **Foreign table**: a table that stores information, such as the source location, format, destination location, encoding format, and data delimiter of a source data file. It is used to associate source data files with the target table.
-  **Target table**: a table in the database. It can be a row-store table or column-store table. Data in the source data files will be imported to this table.

Parallel Import Using GDS
-------------------------

-  If a large volume of data is stored on multiple servers, deploy, configure, and start GDS on each server. Then, data on all the servers can be imported in parallel, as shown in :ref:`Figure 2 <en-us_topic_0000001717256732__en-us_topic_0000001233883381_fig208886410250>`.

   .. _en-us_topic_0000001717256732__en-us_topic_0000001233883381_fig208886410250:

   .. figure:: /_static/images/en-us_image_0000001188163822.png
      :alt: **Figure 2** Parallel import from multiple data servers

      **Figure 2** Parallel import from multiple data servers

   .. important::

      The number of GDS processes cannot exceed that of DNs. If multiple GDS processes are connected to one DN, some of the processes will probably become abnormal.

-  If data is stored on data servers, and both GaussDB(DWS) and the data servers have available I/O resources, you can use GDS for multi-thread concurrent import.

   GDS determines the number of threads based on the number of concurrent import transactions. Even if multi-thread import is configured before GDS startup, the import of a single transaction will not be accelerated. By default, an **INSERT** statement is an import transaction.

Multi-thread concurrent import enables you to:

-  Fully use resources and improve the concurrent import efficiency when you import multiple tables to the database.

-  Speed up the import of a table with a large volume of data.

   Table data is split into multiple data files, and multi-thread concurrent import is implemented by importing data using multiple foreign tables at the same time. Ensure that a data file can be read only by one foreign table.

Import Process
--------------


.. figure:: /_static/images/en-us_image_0000001188482352.png
   :alt: **Figure 3** Concurrent import procedure of GDS

   **Figure 3** Concurrent import procedure of GDS

.. table:: **Table 1** Process description

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Procedure                         | Description                                                                                                                                                                                                                                                                       |
   +===================================+===================================================================================================================================================================================================================================================================================+
   | Prepare source data.              | Prepare the source data files to be imported to the database and upload the files to the data server.                                                                                                                                                                             |
   |                                   |                                                                                                                                                                                                                                                                                   |
   |                                   | For details, see :ref:`Preparing Source Data <en-us_topic_0000001717097308>`.                                                                                                                                                                                                     |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Start GDS.                        | Install, configure, and enable GDS on the data server.                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                   |
   |                                   | For details, see :ref:`Installing, Configuring, and Starting GDS <en-us_topic_0000001764817369>`.                                                                                                                                                                                 |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Create a foreign table.           | A foreign table is used to identify source files. The foreign table stores information, such as the source location, format, destination location, encoding format, and inter-data delimiter of a source data file.                                                               |
   |                                   |                                                                                                                                                                                                                                                                                   |
   |                                   | For details, see :ref:`Creating a GDS Foreign Table <dws_04_0194>`.                                                                                                                                                                                                               |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Import data.                      | After creating the foreign table, run the **INSERT** statement to quickly import data to the target table. For details, see :ref:`Importing Data <en-us_topic_0000001717256736>`.                                                                                                 |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Handle the error table.           | If errors occur during parallel data import, handle errors based on the error information to ensure data integrity.                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                   |
   |                                   | For details, see :ref:`Handling Import Errors <en-us_topic_0000001717097312>`.                                                                                                                                                                                                    |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Improve query efficiency.         | After data is imported, run the **ANALYZE** statement to generate table statistics. The **ANALYZE** statement stores the statistics in the **PG_STATISTIC** system catalog. The execution plan generator uses the statistics to generate the most efficient query execution plan. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Stop GDS.                         | After data import is complete, log in to each data server and stop GDS.                                                                                                                                                                                                           |
   |                                   |                                                                                                                                                                                                                                                                                   |
   |                                   | For details, see :ref:`Stopping GDS <en-us_topic_0000001764817373>`.                                                                                                                                                                                                              |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
