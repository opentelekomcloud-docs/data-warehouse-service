:original_name: dws_04_0266.html

.. _dws_04_0266:

.. _en-us_topic_0000001764650824:

Exporting Data
==============

Prerequisites
-------------

Ensure that the IP addresses and ports of servers where CNs and DNs are deployed can connect to those of the GDS server.

Syntax
------

**Run the following command to export data:**

::

   INSERT INTO  [Foreign table name] SELECT * FROM [Source table name];

.. note::

   Create batch processing scripts to export data in parallel. The degree of parallelism depends on the server resource usage. You can test several tables and monitor resource usage to determine whether to increase or reduce the amount. Common resource monitoring commands include **top** for memory and CPU usage, **iostat** for I/O usage, and **sar** for networks. For details about application cases, see :ref:`Exporting Data Using Multiple Threads <en-us_topic_0000001764650796__en-us_topic_0000001188482140_s855daf73006d4e05ba6d04f8db74e7f6>`.

Examples
--------

-  **Example 1**: Export data from the **reason** table to data files through the **foreign_tpcds_reasons** foreign table.

   ::

      INSERT INTO foreign_tpcds_reasons SELECT * FROM tpcds.reason;

-  **Example 2**: Export part of the data to data files by specifying the filter condition **r_reason_sk =1**.

   ::

      INSERT INTO foreign_tpcds_reasons SELECT * FROM tpcds.reason WHERE r_reason_sk=1;

-  **Example 3**: Data of a special type, such as RAW, is exported as a binary file, which cannot be recognized by the import tool. You need to use the RAWTOHEX() function to convert it to hexadecimal the format before export.

   ::

      INSERT INTO foreign_tpcds_reasons SELECT RAWTOHEX(c) FROM tpcds.reason;
