:original_name: dws_04_1410.html

.. _dws_04_1410:

Proactive Preheating and Tuning of Disk Cache
=============================================

This function is supported only in 9.1.0.200 or later.

Overview
--------

In the storage-compute decoupling architecture, user data is stored in OBS to reduce storage costs. Each query generates network I/Os to retrieve data from OBS. To improve query speed, reduce storage costs, and minimize performance loss, the architecture provides disk cache capability. Pre-queried data is cached locally, enhancing performance.

The LRU2Q algorithm manages the disk cache with three queues: A1in, A1out, and Am. Data first enters A1in. If A1in is full (adjustable via a GUC parameter, default is 0.25 times the total queue size), data moves to A1out. Data enters Am only when hit in A1out. Am queue holds the hottest data.

For common queries, LRU2Q is sufficient. However, frequent joins of large and small tables can degrade performance if small tables are frequently evicted from A1in to A1out.

Tuning Syntax
-------------

A new tuning policy allows directly adding small table data to Am, ensuring it remains hot and reducing network I/Os during joins. The syntax formats are as follows:

#. Perform the actual query and add data directly to Am.

   .. code-block::

      explain warmup hot select ...;

   |image1|

#. Query data in the sequence A1in > A1out > Am.

   .. code-block::

      explain warmup select ...;

   |image2|

#. No actual query operation is performed, and the **explain** logic remains unchanged.

   .. code-block::

      explain select ...;

.. |image1| image:: /_static/images/en-us_image_0000002059501845.png
.. |image2| image:: /_static/images/en-us_image_0000002023418392.png
