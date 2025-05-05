:original_name: dws_04_0125.html

.. _dws_04_0125:

UPDATE and DELETE Operations
============================

.. _en-us_topic_0000002100586270__en-us_topic_0000002100207566_section63531734104519:

Suggestion 3.5: Preventing Simultaneous Updates or Deletions of the Same Row in a Row-store Table
-------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Concurrent **UPDATE** and **DELETE** operations on row-store tables may cause row lock blockage and distributed deadlocks, which can lead to service errors and performance degradation.

   **Solution:**

   -  Group **UPDATE** and **DELETE** operations by primary key or distribution column. Perform parallel operations between groups while keeping operations within a group serial.

.. _en-us_topic_0000002100586270__en-us_topic_0000002100207566_section6619155554517:

Suggestion 3.6: Avoiding Frequent or Simultaneous UPDATE and DELETE Operations on Column-store Tables
-----------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Frequent **UPDATE** and **DELETE** operations on column-store tables can result in CU bloat, leading to large space occupation and decreased access performance.
   -  Concurrent **UPDATE** and **DELETE** operations on row-store tables may cause row lock blockage and distributed deadlocks, which can lead to service errors and performance degradation.

   **Solution:**

   -  Design tables with frequent **UPDATE** and **DELETE** operations as row-store tables.
   -  Group **UPDATE** and **DELETE** operations by primary key or distribution column. Perform parallel operations between groups while keeping operations within a group serial.
