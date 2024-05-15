:original_name: dws_04_0933.html

.. _dws_04_0933:

Connection Pool Parameters
==========================

When a connection pool is used to access the database, database connections are established and then stored in the memory as objects during system running. When you need to access the database, no new connection is established. Instead, an existing idle connection is selected from the connection pool. After you finish accessing the database, the database does not disable the connection but puts it back into the connection pool. The connection can be used for the next access request.

min_pool_size
-------------

**Parameter description**: Specifies the minimum number of connections between a CN's connection pool and another CN/DN.

**Type**: POSTMASTER

**Value range**: an integer ranging from 1 to 65535

**Default value**: **1**

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

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the connections of a connection pool will be reclaimed.
-  **off** indicates that the connections of a connection pool will not be reclaimed.

**Default value**: **on**

enable_force_reuse_connections
-------------------------------

**Parameter description**: Specifies whether a session forcibly reuses a new connection.

**Type**: BACKEND

**Value range**: Boolean

-  **on** indicates that the new connection is forcibly used.
-  **off** indicates that the current connection is used.

**Default value**: **off**

.. note::

   This is a session connection parameter. You are advised not to configure this parameter.

enable_pooler_parallel
-----------------------

**Parameter description**: Specifies whether a CN's connection pool can be connected in parallel mode.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that a CN's connection pool can be connected in parallel mode.
-  **off** indicates that a CN's connection pool cannot be connected in parallel mode.

**Default value**: **on**
