:original_name: dws_04_0266.html

.. _dws_04_0266:

Exporting Data
==============

Prerequisites
-------------

Ensure that the IP addresses and ports of servers where CNs and DNs are deployed can connect to those of the GDS server.

Procedure
---------

#. Export data.

   ::

      INSERT INTO  [Foreign table name] SELECT * FROM [Source table name];

   .. note::

      Create batch processing scripts to export data in parallel. The degree of parallelism depends on the server resource usage. You can test several tables and monitor resource usage to determine whether to increase or reduce the amount. Common resource monitoring commands include **top** for memory and CPU usage, **iostat** for I/O usage, and **sar** for networks. For details about application cases, see :ref:`Exporting Data Using Multiple Threads <en-us_topic_0000001098974516__s855daf73006d4e05ba6d04f8db74e7f6>`.
