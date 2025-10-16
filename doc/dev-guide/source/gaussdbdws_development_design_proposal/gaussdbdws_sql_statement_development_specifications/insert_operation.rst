:original_name: dws_04_0116.html

.. _dws_04_0116:

INSERT Operation
================

.. _en-us_topic_0000002136265445__en-us_topic_0000002135726605_section1588193713503:

Rule 3.3: Replacing INSERT with COPY for Efficient Multi-Value Batch Insertion
------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Parsing multiple values is time-consuming and resource-intensive, leading to low efficiency when importing data into the database.

   **Solution:**

   -  Instead of using **INSERT VALUES**, the frontend should use APIs like CopyManager of JDBC.

.. _en-us_topic_0000002136265445__en-us_topic_0000002135726605_section2595105519505:

Suggestion 3.4: Avoiding Performing Real-time INSERT Operations on Common Column-store Tables
---------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Importing a small batch of data in real-time to a common column-store table can significantly expand the small CU, occupying a lot of storage space and impacting the query performance.

   **Solution:**

   -  In real-time **INSERT** scenarios, evaluate the amount of data to be imported at once and the total amount of data. If the total amount of data is small, use row-store tables.
   -  In the real-time INSERT scenario, import around 60,000 data records to a single table, partition, or DN at a time. The minimum import batch is 5,000 data records.
   -  In the real-time **INSERT** scenario, use H-Store column-store tables (for clusters of version 8.3.0 or later).
