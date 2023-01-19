:original_name: dws_04_0190.html

.. _dws_04_0190:

Importing Data In Parallel Using GDS
====================================

**INSERT** and **COPY** statements are serially executed to import a small volume of data. To import a large volume of data to GaussDB(DWS), you can use GDS to import data in parallel using a foreign table.

In the current GDS version, you can import data to databases from pipe files.

-  If the local disk space of the GDS user is insufficient:

   -  Data in HDFS can be directly written to a pipe file without occupying extra disk space.

-  If you need to cleanse data before importing data:

   -  You can compile a program as needed and write the data to be processed into a pipe file.

   .. note::

      -  The current version does not support data import through GDS in SSL mode. Do not use GDS in SSL mode.
      -  All pipe files mentioned in this section refer to named pipes on Linux.

Overview
--------

You can import data in parallel from the common file system (excluding HDFS) of a server to GaussDB(DWS).

Data files to be imported are specified based on the import policy and data formats set in a foreign table. Data is imported in parallel through multiple DNs from source data files to the database, which improves the overall data import performance. :ref:`Figure 1 <en-us_topic_0000001099134954__fc07608abaf6e4b509fbbfe774a8a548e>` shows an example.

-  The CN only plans data import tasks and delivers the tasks to DNs. Then the CN is released to process other tasks.
-  In this way, the computing capability and bandwidth of all the DNs are fully leveraged to improve the data import performance.

You can pre-process data, such as replacing invalid characters and processing fault tolerance, by configuring parameters in a foreign table.

.. _en-us_topic_0000001099134954__fc07608abaf6e4b509fbbfe774a8a548e:

.. figure:: /_static/images/en-us_image_0000001099135208.png
   :alt: **Figure 1** Importing data in parallel

   **Figure 1** Importing data in parallel

The concepts mentioned in the preceding figure are described as follows:

-  **CN**: Coordinator of GaussDB(DWS). After receiving import SQL requests from an application or client, the CN plans import tasks and delivers the tasks to DNs.
-  **DN**: Datanode of GaussDB(DWS). After receiving the import tasks delivered by the CN, DNs import data from the source data file to the target table in the database using a foreign table.
-  **Source data file**: a file that stores data to be imported.
-  **Data server**: a server that stores source data files. For security purposes, it is recommended that the data server and GaussDB(DWS) be on the same intranet.
-  **Foreign table**: a table that stores information of a source data file, such as location, format, destination location, encoding format, and data delimiter. It is used to associate source data files with the target table.
-  **Target table**: a table in the database. It can be a row-store table or column-store table. Data in the source data files will be imported to this table.

Parallel Import Using GDS
-------------------------

-  If a large volume of data is stored on multiple servers, install, configure, and start GDS on each server. Then, data on all the servers can be imported in parallel, as shown in :ref:`Figure 2 <en-us_topic_0000001099134954__fig208886410250>`.

   .. _en-us_topic_0000001099134954__fig208886410250:

   .. figure:: /_static/images/en-us_image_0000001098815222.png
      :alt: **Figure 2** Parallel import from multiple data servers

      **Figure 2** Parallel import from multiple data servers

   .. important::

      The number of GDS processes cannot exceed that of DNs. If multiple GDS processes are connected to one DN, some of the processes may become abnormal.

-  If data is stored on one data server and both GaussDB(DWS) and the data server have available I/O resources, you can use GDS for multi-thread concurrent import.

   GDS determines the number of threads based on the number of concurrent import transactions. That is, even if multi-thread import is configured before GDS startup, the import of a single transaction will not be accelerated. By default, an **INSERT** statement is an import transaction.

   Multi-thread concurrent import enables you to:

   -  Fully use resources and improve the concurrent import efficiency when you import multiple tables to the database.

   -  Speed up the import of a table with a large volume of data.

      Table data is split into multiple data files, and multi-thread concurrent import is implemented by importing data using multiple foreign tables at the same time. Ensure that a data file can be read only by one foreign table.

Import Process
--------------


.. figure:: /_static/images/en-us_image_0000001145895199.png
   :alt: **Figure 3** Concurrent import process using GDS

   **Figure 3** Concurrent import process using GDS

.. table:: **Table 1** Process description

   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Process                           | Description                                                                                                                                                                                                                                                                      |
   +===================================+==================================================================================================================================================================================================================================================================================+
   | Prepare source data.              | Prepare the source data files to be imported to the database and upload the files to the data server.                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                  |
   |                                   | For details, see :ref:`Preparing Source Data <dws_04_0192>`.                                                                                                                                                                                                                     |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Start GDS.                        | Install, configure, and start GDS on the data server.                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                  |
   |                                   | For details, see :ref:`Installing, Configuring, and Starting GDS <dws_04_0193>`.                                                                                                                                                                                                 |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Create a foreign table.           | A foreign table is used to identify source files. The foreign table stores information of a source data file, such as location, format, destination location, encoding format, and data delimiter.                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                  |
   |                                   | For details, see :ref:`Creating a GDS Foreign Table <dws_04_0194>`.                                                                                                                                                                                                              |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Import data.                      | After creating the foreign table, run the **INSERT** statement to quickly import data to the target table. For details, see :ref:`Importing Data <dws_04_0195>`.                                                                                                                 |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Handle import errors.             | If errors occur during parallel data import, handle errors based on the error information to ensure data integrity.                                                                                                                                                              |
   |                                   |                                                                                                                                                                                                                                                                                  |
   |                                   | For details, see :ref:`Handling Import Errors <dws_04_0196>`.                                                                                                                                                                                                                    |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Improve query efficiency.         | After data is imported, run the **ANALYZE** statement to generate table statistics. The **ANALYZE** statement stores the statistics in the **PG_STATISTIC** system catalog. When you run the plan generator, the statistics help you generate an efficient query execution plan. |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Stop GDS.                         | After data is imported, log in to each data server and stop GDS.                                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                                                                                                  |
   |                                   | For details, see :ref:`Stopping GDS <dws_04_0197>`.                                                                                                                                                                                                                              |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
