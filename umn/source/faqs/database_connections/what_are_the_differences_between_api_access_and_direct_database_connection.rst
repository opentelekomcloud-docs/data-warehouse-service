:original_name: dws_03_0004.html

.. _dws_03_0004:

What Are the Differences Between API Access and Direct Database Connection?
===========================================================================

You can compare the two connection methods from the perspectives of security and performance.

Security
--------

.. table:: **Table 1** Security comparison

   +--------------------------+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Dimension                | API Access                                                                              | Direct Database Access                                                                                                      |
   +==========================+=========================================================================================+=============================================================================================================================+
   | Communication encryption | HTTPS and TLS encryption are provided.                                                  | SSL must be manually configured.                                                                                            |
   +--------------------------+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Identity authentication  | AK/SK or token authentication is supported, which is more flexible.                     | Database account password authorization is used. The permission granularity is coarse (for example, schema or table level). |
   +--------------------------+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Access control           | The IP address, frequency, and blacklist can be restricted at the API gateway layer.    | Whitelist is used for managing database networks (such as VPCs and security groups).                                        |
   +--------------------------+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | SQL injection prevention | Parameter-based APIs can naturally prevent SQL injection.                               | Code is used to prevent SQL injection.                                                                                      |
   +--------------------------+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Audit and logging        | API calling logs (request sources and parameters) are recorded in a centralized manner. | There are database logs (SQL statements and source IP addresses).                                                           |
   +--------------------------+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+

API access is more secure especially when your database is exposed to external systems. Direct connection to your database requires additional security configurations.

Performance
-----------

.. table:: **Table 2** Performance comparison

   +-----------------------+----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
   | Dimension             | API Access                                                                                               | Direct Database Access                                                            |
   +=======================+==========================================================================================================+===================================================================================+
   | Network overhead      | Additional HTTP protocol headers are required, and the serialization and deserialization costs are high. | Efficient binary protocols (such as libpq) are used.                              |
   +-----------------------+----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
   | Latency               | Higher delay (To access a database, the API gateway and application server need to be accessed first).   | Lower latency (direct communication with a database)                              |
   +-----------------------+----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
   | Throughput            | The throughput is limited by the API server performance.                                                 | The high-performance database engine is directly used to deploy high throughput.  |
   +-----------------------+----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
   | Long connection reuse | Short connections are usually used, because connections need to be frequently established.               | Using connection pools helps to alleviate connection management overhead.         |
   +-----------------------+----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
   | Complex queries       | Complex SQL statements cannot be directly transmitted.                                                   | Native SQL is supported, and the optimizer can maximize the database performance. |
   +-----------------------+----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+

Direct database connection is more efficient, especially in complex query or high-throughput scenarios. API access is suitable for lightweight, high-frequency simple requests.

Scenarios
---------

The two methods are suitable for different scenarios.

-  Direct database connection is better for operations on a large amount of data. The security is not high. You need to provide the database name, account, and password.
-  API access is an ideal choice for low-frequency access and fixed operations. It features high security, requires a service layer, and supports field control.
