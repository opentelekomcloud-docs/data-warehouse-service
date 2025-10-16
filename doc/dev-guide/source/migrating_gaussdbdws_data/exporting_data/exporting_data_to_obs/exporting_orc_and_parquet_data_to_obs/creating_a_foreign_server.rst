:original_name: dws_04_0259.html

.. _dws_04_0259:

Creating a Foreign Server
=========================

Create an OBS foreign server and an HDFS foreign server.

Create an OBS external server to define the OBS server information for the foreign table. The creation method varies by cluster version. For details, see :ref:`Creating a Foreign Server <dws_04_0244>`.

-  For 8.2.0 and later versions, delegate access to OBS bucket data by creating an OBS data source on the OBS console.
-  For versions before 8.2.0, you will need to manually create an OBS server. For details about the syntax for creating foreign servers, see CREATE SERVER.

For how to create an HDFS foreign server, see :ref:`Manually Creating a Foreign Server <dws_04_0213>`.
