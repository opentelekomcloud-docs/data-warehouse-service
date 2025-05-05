:original_name: dws_04_0083.html

.. _dws_04_0083:

GaussDB(DWS) JDBC Configuration Rules
=====================================

Currently, third-party tools are connected to GaussDB(DWS) trough JDBC. This section describes the precautions for configuring the tools.

Connection Parameters
---------------------

-  [Notice] When a third-party tool connects to GaussDB(DWS) through JDBC, JDBC sends a connection request to GaussDB(DWS). By default, the following parameters are added. For details, see the implementation of the ConnectionFactoryImpl JDBC code.

   .. code-block::

      params = {
      { "user", user },
      { "database", database },
      { "client_encoding", "UTF8" },
      { "DateStyle", "ISO" },
      { "extra_float_digits", "2" },
      { "TimeZone",  createPostgresTimeZone() },
      };

   These parameters may cause the JDBC and gsql clients to display inconsistent data, for example, date data display mode, floating point precision representation, and timezone.

   If the result is not as expected, you are advised to explicitly set these parameters in the Java connection setting.

-  [Proposal] When connecting to the database through JDBC, ensure that the following two time zones are the same:

   -  Time zone of the host where the JDBC client is located
   -  Time zone of the host where the GaussDB(DWS) server is located

fetchsize
---------

[Notice] To use **fetchsize** in applications, disable the **autocommit** switch. Enabling the **autocommit** switch makes the **fetchsize** configuration invalid.

autocommit
----------

[Proposal] It is recommended that you enable the **autocommit** switch in the code for connecting to GaussDB(DWS) by the JDBC. If **autocommit** needs to be disabled to improve performance or for other purposes, applications need to ensure their transactions are committed. For example, explicitly commit translations after specifying service SQL statements. Particularly, ensure that all transactions are committed before the client exits.

Connection Releasing
--------------------

[Proposal] You are advised to use connection pools to limit the number of connections from applications. Do not connect to a database every time you run an SQL statement.

[Proposal] After an application completes its tasks, disconnect its connection to GaussDB(DWS) to release occupied resources. You are advised to set the session timeout interval in the task.

[Proposal] Reset the session environment before releasing connections to the JDBC connection tool. Otherwise, historical session information may cause object conflicts.

-  If GUC parameters are set in the connection, before you return the connection to the connection pool, run **SET SESSION AUTHORIZATION DEFAULT;RESET ALL;** to clear the connection status.
-  If a temporary table is used, delete it before you return the connection to the connection pool.

CopyManager
-----------

[Proposal] In the scenario where the ETL tool is not used and real-time data import is required, it is recommended that you use the CopyManager interface driven by the GaussDB(DWS) JDBC to import data in batches during application development.

For how to use CopyManager, see :ref:`CopyManager <en-us_topic_0000002080099797__en-us_topic_0000001188642156_section63487150165>`.
