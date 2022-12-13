:original_name: dws_03_0005.html

.. _dws_03_0005:

What Are the Differences Between a Data Warehouse and the Hadoop Big Data Platform?
===================================================================================

The Hadoop big data platform can be regarded as a next-generation data warehousing system. It has the characteristics of modern data warehouses and is widely used by enterprises. Because of the scalability of MPP, the MPP-based data warehousing system is sometimes classified as a big data platform.

However, data warehouses greatly differ from the Hadoop platform in function and user experience in different scenarios. For details, see the following table.

.. table:: **Table 1** Feature comparison between data warehouses and the Hadoop big data platform

   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Feature                     | Hadoop                                                               | Data Warehouse             |
   +=============================+======================================================================+============================+
   | Number of compute nodes     | 1000s                                                                | Max 256                    |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Data volume                 | Over 10 PB                                                           | Max 10 PB                  |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Data type                   | Relational, semi-relational, unstructured (voice, images, and video) | Relational only            |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Latency                     | Medium to high                                                       | Low                        |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Application ecosystem       | Innovative/AI                                                        | Traditional/BI             |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Application development API | SQL and other programming language APIs, such as MapReduce           | Standard database SQL      |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Scalability                 | Unlimited, with comprehensive programming APIs                       | Limited, supported by UDFs |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+
   | Transaction support         | Limited                                                              | Comprehensive              |
   +-----------------------------+----------------------------------------------------------------------+----------------------------+

Data warehouses and the Hadoop platform work together in different scenarios. GaussDB(DWS) on the public cloud can seamlessly integrate with Hadoop-based MRS on the public cloud to provide the SQL-over-Hadoop data sharing across platforms and services. GaussDB(DWS) serves as a data warehouse for managing massive data while relishing the openness, convenience, and innovation of the Hadoop platform. You can also enjoy the upper-layer applications of conventional data warehouses, especially BI applications, using GaussDB(DWS).
