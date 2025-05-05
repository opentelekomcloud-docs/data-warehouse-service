:original_name: dws_04_0933.html

.. _dws_04_0933:

Connection Pool Parameters
==========================

When a connection pool is used to access the database, database connections are established and then stored in the memory as objects during system running. When you need to access the database, no new connection is established. Instead, an existing idle connection is selected from the connection pool. After you finish accessing the database, the database does not disable the connection but puts it back into the connection pool. The connection can be used for the next access request.

max_pool_size
-------------

**Parameter description**: Specifies the maximum number of connections between a CN's connection pool and another CN/DN.

**Type**: POSTMASTER

**Value range**: an integer ranging from 1 to 65535

**Default value**: **800** for CNs and **5000** for DNs

persistent_datanode_connections
-------------------------------

**Parameter description**: Specifies whether to release the connection for the current session.

**Type**: USERSET

**Value range**: Boolean

-  **off** indicates that the connection for the current session will be released.
-  **on** indicates that the connection for the current session will not be released.

   .. important::

      After this function is enabled, a session may hold a connection but does not run a query. As a result, other query requests fail to be connected. To fix this problem, the number of sessions must be less than or equal to **max_active_statements**.

**Default value**: **off**

cache_connection
----------------

**Parameter description**: Specifies whether to reclaim the connections of a connection pool.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the connections of a connection pool will be reclaimed.
-  **off** indicates that the connections of a connection pool will not be reclaimed.

**Default value**: **on**

enable_force_reuse_connections
-------------------------------

**Parameter description**: Specifies whether a session forcibly reuses a new connection.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the new connection is forcibly used.
-  **off** indicates that the current connection is used.

**Default value**: **off**

syscache_clean_policy
---------------------

**Parameter description**: Specifies the policy for clearing the memory and number of idle DN connections. This is supported only by clusters of version 9.1.0.100 or later.

**Type**: SIGHUP

**Value range**: a string

This parameter policy consists of three values:

#. The first value ranges from 0 to 1 and represents the percentage of total available memory used by DNs. When the percentage of used memory reaches this value, 1/4 of the stream threads will be cleared, and the second value will be evaluated.
#. The second value ranges from 0 to 1 and represents the percentage of total available memory used by syscache on DNs. When the percentage of syscache memory usage reaches this value, the third value will be evaluated.
#. The third value ranges from 0 to INT_MAX and is measured in MB. It represents the size of syscache memory used by idle threads. When the syscache memory usage of an idle thread reaches this value, the syscache used by that thread will be cleared.

**Default value**: 0.8,0.3,64

.. important::

   -  Before setting this parameter, evaluate the memory usage using views :ref:`PV_SESSION_MEMORY_DETAIL <dws_04_0852>` and :ref:`PV_TOTAL_MEMORY_DETAIL <dws_04_0855>`.
   -  When setting this parameter, follow the specified format, ensuring that the three values are separated by commas without spaces.
   -  If the parameter is not set according to the specified format and the setting fails, a WARNING log will be generated in the log, and the parameter value displayed when using the SHOW command to query the parameter will be the last successfully set value. If the setting fails and the system is restarted, the parameter will be set to the default value.
   -  During the Readcommand phase, if a thread on CN times out after 30 seconds, it will clear DNs if syscache is greater than 256 MB. There are two operations:

      #. If the overall memory usage reaches 80%, an auxiliary thread will monitor the memory usage and clear 1/4 of the stream threads. It will also check if syscache usage exceeds 30% of the total memory usage. If it does, it will clear the syscache of Readcommand phase pg threads greater than 64 MB.
      #. If a stream thread is idle for more than 30 seconds and syscache usage is greater than 64 MB, it will clear the syscache.
