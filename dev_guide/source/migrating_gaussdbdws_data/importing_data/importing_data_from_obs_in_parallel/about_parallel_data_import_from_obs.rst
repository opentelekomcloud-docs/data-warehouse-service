:original_name: dws_04_0182.html

.. _dws_04_0182:

About Parallel Data Import from OBS
===================================

The object storage service (OBS) is an object-based cloud storage service, featuring data storage of high security, proven reliability, and cost-effectiveness. OBS provides large storage capacity for you to store files of any type.

GaussDB(DWS), a data warehouse service, uses OBS as a platform for converting cluster data and external data, satisfying the requirements for secure, reliable, and cost-effective storage.

You can import data in text, CSV, ORC, Parquet, CarbonData, or JSON format from OBS to GaussDB(DWS) for query, and can remotely read data from OBS. You are advised to import frequently accessed hot data to GaussDB(DWS) to facilitate queries and store cold data to OBS for remote read to reduce cost.

Currently, data can be imported using either of the following methods:

-  Method 1: You do not need to create a server. Use the default server to create a foreign table. Data in TXT or CSV format is supported. For details, see :ref:`Importing CSV/TXT Data from OBS <dws_04_0154>`.
-  Method 2: You need to create a server and use the server to create a foreign table. Data in ORC, CarbonData, text, CSV, Parquet, or JSON format is supported. For details, see :ref:`Importing ORC, CarbonData, Parquet, and JSON Data from OBS <dws_04_0155>`.

.. important::

   -  Ensure that no Chinese characters are contained in paths used for importing data to or exporting data from OBS.

   -  Data cannot be imported to or exported from OBS across regions. Ensure that OBS and the GaussDB(DWS) cluster are in the same region.

   -  To ensure the correctness of data import or export, you need to import or export data from OBS in the same compatibility mode.

      If data is imported or exported in MySQL compatibility mode, it can only be exported or imported in the same mode.

Overview
--------

During data migration and Extract-Transform-Load (ETL), a massive volume of data needs to be imported to GaussDB(DWS) in parallel. The common import mode is time-consuming. When you import data in parallel using OBS foreign tables, source data files to be imported are identified based on the import URL and data formats specified in the tables. Data is imported in parallel through DNs to GaussDB(DWS), which improves the overall import performance.

**Advantages:**

-  The CN only plans and delivers data import tasks, and the DNs execute these tasks. This reduces CN resource usage, enabling the CN to process external requests.
-  In this way, the computing capabilities and bandwidths of all the DNs are fully leveraged to import data.
-  You can preprocess data before the import.
-  Fault tolerance can be configured for data format errors during the data import. You can locate incorrect data based on displayed error information after the data is imported.

**Disadvantage:**

You need to create OBS foreign tables and store to-be-imported data on OBS.

**Application Scenario**:

A large volume of local data is imported concurrently on many DNs.

Related Concepts
----------------

-  **Source data file**: a TEXT, CSV, ORC, CARBONDATA, or JSON file that stores data to be imported in parallel.

-  **OBS**: a cloud storage service used to store unstructured data, such as documents, images, and videos. Data is imported in parallel from the OBS server to GaussDB(DWS).

-  **Bucket**: a container storing objects on OBS.

   -  Object storage is a flat storage mode. Layered file system structures are not needed because all objects in buckets are at the same logical layer.
   -  In OBS, each bucket name must be unique and cannot be changed. A default access control list (ACL) is created with a bucket. Each item in the ACL contains permissions granted to certain users, such as **READ**, **WRITE**, and **FULL_CONTROL**. Only authorized users can perform bucket operations, such as creating, deleting, viewing, and setting ACLs for buckets.
   -  A user can create a maximum of 100 buckets. The total data size and the number of objects and files in each bucket are not limited.

-  **Object**: a basic data storage unit in OBS. Data uploaded by users is stored in OBS buckets as objects. Object attributes include **Key**, **Metadata**, and **Data**.

   Generally, objects are managed as files. However, OBS has no file system-related concepts, such as files and folders. To let users easily manage data, OBS allows them to simulate folders. Users can add a slash (/) in the object name, for example, **tpcds1000/stock.csv**. In this name, **tpcds1000** is regarded as the folder name and **stock.csv** the file name. The value of **Key** (object name) is still **tpcds1000/stock.csv**, and the content of the object is the content of the **stock.csv** file.

-  **Key**: name of an object. It is a UTF-8 character sequence containing 1 to 1024 characters. A key value must be unique in a bucket. Users can name the objects they stored or obtained as *Bucket name*\ **+**\ *Object name*.

-  **Metadata**: object metadata, which contains information about the object. There are system metadata and user metadata. The metadata is uploaded to OBS as key-value pairs together with HTTP headers.

   -  System metadata is generated by OBS and used for processing object data. System metadata includes **Date**, **Content-length**, **last-modify**, and **Content-MD5**.
   -  User metadata contains object descriptions specified by users for uploading objects.

-  **Data**: object content. OBS does not sense the content and regards it as stateless binary data.

-  **Ordinary table**: A database table that stores data imported to data files in parallel. Ordinary tables are classified into row-store tables and column-store tables.

-  **Foreign table**: A foreign table is used to identify data in a source data file. The foreign table stores information, such as the location, format, encoding, and inter-data delimiter of a source data file.

.. _en-us_topic_0000001764650604__en-us_topic_0000001233883255_en-us_topic_0000001082926905_en-us_topic_0117407705_sefc365e1804e4606aafdeb3398080e73:

How Data Is Imported
--------------------

:ref:`Figure 1 <en-us_topic_0000001764650604__en-us_topic_0000001233883255_en-us_topic_0000001082926905_en-us_topic_0117407705_fb5a2ab52cbc942ce89ac96237380de66>` shows how data is imported from OBS. The CN plans and delivers data import tasks. It delivers tasks to each DN by file.

The delivery method is as follows:

In :ref:`Figure 1 <en-us_topic_0000001764650604__en-us_topic_0000001233883255_en-us_topic_0000001082926905_en-us_topic_0117407705_fb5a2ab52cbc942ce89ac96237380de66>`, there are four DNs (DN0 to DN3) and OBS stores six files numbered from t1.data.0 to t1.data.5. The files are delivered as follows:

t1.data.0 -> DN0

t1.data.1 -> DN1

t1.data.2 -> DN2

t1.data.3 -> DN3

t1.data.4 -> DN0

t1.data.5 -> DN1

Two files are delivered to DN0 and DN1, respectively. One file is delivered to each of the other DNs.

The import performance is the best when one OBS file is delivered to each DN and all the files have the same size. To improve the performance of loading data from OBS, split the data file into multiple files as evenly as possible before storing it to OBS. The recommended number of split files is an integer multiple of the DN quantity.

.. _en-us_topic_0000001764650604__en-us_topic_0000001233883255_en-us_topic_0000001082926905_en-us_topic_0117407705_fb5a2ab52cbc942ce89ac96237380de66:

.. figure:: /_static/images/en-us_image_0000002221491198.jpg
   :alt: **Figure 1** Parallel data import using OBS foreign tables

   **Figure 1** Parallel data import using OBS foreign tables

Import Flowchart
----------------


.. figure:: /_static/images/en-us_image_0000002256410929.png
   :alt: **Figure 2** Parallel import procedure

   **Figure 2** Parallel import procedure

.. table:: **Table 1** Procedure description

   +------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Procedure                    | Description                                                                                                                                                                                                                                                                      | Subtask               |
   +==============================+==================================================================================================================================================================================================================================================================================+=======================+
   | Upload data to OBS.          | Plan the storage path on the OBS server and upload data files.                                                                                                                                                                                                                   | ``-``                 |
   |                              |                                                                                                                                                                                                                                                                                  |                       |
   |                              | For details, see :ref:`Uploading Data to OBS <dws_04_0184>`.                                                                                                                                                                                                                     |                       |
   +------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Create an OBS foreign table. | Create a foreign table to identify source data files on the OBS server. The OBS foreign table stores data source information, such as its bucket name, object name, file format, storage location, encoding format, and delimiter.                                               | ``-``                 |
   |                              |                                                                                                                                                                                                                                                                                  |                       |
   |                              | For details, see :ref:`Creating an OBS Foreign Table <dws_04_0185>`.                                                                                                                                                                                                             |                       |
   +------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Import data.                 | After creating the foreign table, run the **INSERT** statement to efficiently import data to the target tables.                                                                                                                                                                  | ``-``                 |
   |                              |                                                                                                                                                                                                                                                                                  |                       |
   |                              | For details, see :ref:`Importing Data <dws_04_0186>`.                                                                                                                                                                                                                            |                       |
   +------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Handle import errors.        | If errors occur during data import, handle them based on the displayed error information described in :ref:`Handling Import Errors <dws_04_0187>` to ensure data integrity.                                                                                                      | ``-``                 |
   +------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Improve query efficiency.    | After data is imported, run the **ANALYZE** statement to generate table statistics. The **ANALYZE** statement stores the statistics in the **PG_STATISTIC** system catalog. When you run the plan generator, the statistics help you generate an efficient query execution plan. | ``-``                 |
   +------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
