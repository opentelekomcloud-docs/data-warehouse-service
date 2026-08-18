:original_name: dws_04_1070.html

.. _dws_04_1070:

Introduction to Hudi
====================

SQL on Hudi of DWS is supported only in 9.1.0.100 and later versions.

Apache Hudi indicates Hadoop Upserts Deletes and Incrementals. It is used to manage large analytical datasets stored in the distributed file system (DFS) of the Hadoop big data system.

Hudi is not just a data format. It is also a set of data access methods (similar to the access layer of DWS storage). In Apache Hudi 0.9, big data components such as Spark and Flink have their own clients. The following figure shows the logical storage of Hudi.

|image1|

-  Write Mode

   **COW**: copy-on-write, which is suitable for scenarios where updates are rare.

   **MOR**: replication on read. For UPDATE & DELETE, delta log files are written incrementally. During analysis, base and delta log files are compacted asynchronously.

-  Storage Format

   **index**: index of the primary key. The default value is bloomfilter at the file group level.

   **data files**: base file + delta log file (for updating and deleting base files)

   **timeline metadata**: manages version logs.

-  Views

   Read-optimized view: reads the base file generated after compaction. The reading of data that is not compacted has some latency (efficient read).

   Real-time view: reads the latest data. The base file and delta file are combined during the read (frequent updates).

   Incremental view: reads the incremental data written to Hudi, similar to CDC (stream and batch integration).

.. |image1| image:: /_static/images/en-us_image_0000001811491577.png
