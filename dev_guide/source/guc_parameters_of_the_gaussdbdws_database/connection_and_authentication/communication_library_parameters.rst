:original_name: dws_04_0891.html

.. _dws_04_0891:

Communication Library Parameters
================================

This section describes parameter settings and value ranges for communication libraries.

tcp_keepalives_idle
-------------------

**Parameter description**: Specifies the interval between keepalive signal sending in an OS that supports the **TCP_KEEPIDLE** socket parameter. If no keepalive signal is transmitted, the connection is in idle state.

**Type**: USERSET

.. important::

   -  If the OS does not support the **TCP_KEEPIDLE** parameter, set this parameter to **0**.
   -  The parameter is ignored on the OS where connections are established using the Unix domain socket.

**Value range**: an integer ranging from 0 to 3600. The unit is second (s).

**Default value**: **0**

tcp_keepalives_interval
-----------------------

**Parameter description:** Specifies the response time before retransmission when the OS supports the **TCP_KEEPINTVL** socket parameter.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 180. The unit is second (s).

**Default value**: **0**

.. important::

   -  If the OS does not support the **TCP_KEEPINTVL** parameter, set this parameter to **0**.
   -  The parameter is ignored on the OS where connections are established using the Unix domain socket.

tcp_keepalives_count
--------------------

**Parameter description**: Specifies the number of keepalived signals that can be waited before the GaussDB(DWS) server is disconnected from the client if the OS supports the **TCP_KEEPCNT** socket parameter.

**Type**: USERSET

.. important::

   -  If the OS does not support the **TCP_KEEPCNT** parameter, set this parameter to **0**.
   -  The parameter is ignored on the OS where connections are established using the Unix domain socket.

**Value range**: an integer ranging from 0 to 100. **0** indicates that the connection is immediately broken if GaussDB(DWS) does not receive a keepalived signal from the client.

**Default value**: **0**

.. _en-us_topic_0000001764491904__s0011acc521b848fe85b25a68f534dc73:

comm_max_datanode
-----------------

**Parameter description**: Specifies the maximum number of DNs supported by the communication library.

**Type**: USERSET

**Value range**: an integer ranging from 1 to 8192

**Default value**: actual number of DNs

.. important::

   Increasing this parameter value takes effect immediately, while decreasing the value takes effect after the cluster is restarted.

comm_max_stream
---------------

**Parameter description**: maximum number of logical connection data structures cached in the communication library.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 65535

**Default value**: **1024**

.. note::

   If the value of :ref:`comm_max_datanode <en-us_topic_0000001764491904__s0011acc521b848fe85b25a68f534dc73>` is small, the process memory is sufficient. In this case, you can increase the value of **comm_max_stream**.

max_stream_pool
---------------

**Parameter description**: Specifies the maximum number of stream threads that can be contained in a stream thread pool. This feature is supported in 8.1.2 or later.

**Type**: SUSET

**Value range**: an integer ranging from -1 to INT_MAX. The values **-1** and **0** indicate that the stream thread pool is disabled.

**Default value**:

-  The formula for a new cluster is **max_stream_pool=MIN(max_connections, max_process_memory/16/5MB, 1024)**.
-  The formula for a cluster upgraded from versions earlier than is **max_stream_pool = MIN(max_connections, max_process_memory/16/5MB, 1024, value of the old cluster)**. During the upgrade, the settings for the new cluster are forcibly used. The old value is used if it is smaller.

.. note::

   -  The number of stream threads in a thread pool can be reduced in real time. If the value of this parameter is increased, the number of stream threads is increased to meet the service requirements.
   -  Generally, you are advised not to change the value of this parameter because the stream thread pool supports the automatic cleanup function.
   -  If too many idle stream threads occupy the memory, you can decrease the value of this parameter to save the memory.

enable_stream_sync_quit
-----------------------

**Parameter description**: whether the stream threads exit synchronously when the stream plan ends. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that threads in the stream thread group exit after the stream plan ends.
-  **off** indicates that stream threads exit directly after the stream plan ends without waiting for the threads in the stream thread group to exit.

**Default value**: **off**

enable_connect_standby
----------------------

**Parameter description**: Sets the connection between a CN and a standby DN. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the CN connects to the standby server.
-  **off** indicates that the CN connects to the primary DN.

**Default value**: **off**

.. caution::

   -  You are not advised to use this parameter in routine services. This parameter applies only to O&M operations. You are not advised to use the **gs_guc tool** for global settings. Otherwise, problems such as data inconsistency and result set errors may occur.
   -  Enabling this parameter for a session with temporary tables will delete the temporary table data on DNs and prevent further actions on those tables.

comm_quota_size
---------------

**Parameter description**: Specifies the maximum size of packets that can be continuously sent by the communication library. When you use a 1GE NIC, a small value ranging from 20 KB to 40 KB is recommended.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 102400. The default unit is KB. The value **0** indicates that the quota mechanism is not used.

**Default value**: **1 MB**

comm_usable_memory
------------------

**Parameter description**: Specifies the maximum memory that can be used by the communication library cache on a single DN.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 256. The default unit is KB. The minimum size cannot be less than 1 GB for installation.

**Default value**: **max_process_memory/8**

.. important::

   This parameter must be specifically set based on environment memory and the deployment method. If it is too large, out-of-memory (OOM) may occur. If it is too small, the performance of the communication library may deteriorate.

comm_client_bind
----------------

**Parameter description**: Specifies whether to bind the client of the communication library to a specified IP address when the client initiates a connection.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the client is bound to a specified IP address.
-  **off** indicates that the client is not bound to any IP addresses.

.. important::

   If multiple IP addresses of a node in a cluster are on the same communication network segment, set this parameter to **on**. In this case, the client is bound to the IP address specified by **listen_addresses**. The concurrency performance of a cluster depends on the number of random ports because a port can be used only by one client at a time.

**Default value**: **off**

comm_no_delay
-------------

**Parameter description**: Specifies whether to use the **NO_DELAY** attribute of the communication library connection. Restart the cluster for the setting to take effect.

**Type**: USERSET

**Value range**: Boolean

**Default value:** **off**

.. important::

   If packet loss occurs because a large number of packets are received per second, set this parameter to **off** to reduce the total number of packets.

comm_debug_mode
---------------

**Parameter description**: Specifies the debug mode of the communication library, that is, whether to print logs about the communication layer. The setting is effective at the session layer.

.. important::

   When the switch is set to **on**, the number of printed logs is huge, adding extra overhead and reducing database performance. Therefore, set the switch to **on** only in the debug mode.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the detailed debug log of the communication library is printed.
-  **off** indicates the detailed debug log of the communication library is not printed.

**Default value**: **off**

comm_ackchk_time
----------------

**Parameter description**: Specifies the duration after which the communication library server automatically triggers ACK when no data package is received.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 20000. The unit is millisecond (ms). **0** indicates that automatic ACK triggering is disabled.

**Default value**: **2000**

comm_timer_mode
---------------

**Parameter description**: Specifies the timer mode of the communication library, that is, whether to print timer logs in each phase of the communication layer. The setting is effective at the session layer.

.. important::

   When the switch is set to **on**, the number of printed logs is huge, adding extra overhead and reducing database performance. Therefore, set the switch to **on** only in the debug mode.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the detailed timer log of the communication library is printed.
-  **off** indicates the detailed timer log of the communication library is not printed.

**Default value**: **off**

comm_stat_mode
--------------

**Parameter description**: Specifies the stat mode of the communication library, that is, whether to print statistics about the communication layer. The setting is effective at the session layer.

.. important::

   When the switch is set to **on**, the number of printed logs is huge, adding extra overhead and reducing database performance. Therefore, set the switch to **on** only in the debug mode.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the statistics log of the communication library is printed.
-  **off** indicates the statistics log of the communication library is not printed.

**Default value:** **off**

client_connection_check_interval
--------------------------------

**Parameter description**: Specifies the interval for checking the client connection status. This parameter is supported by clusters of version 8.2.0 or later.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is ms. The value **0** indicates that the client connection status is not checked.

**Default value**: **10000**

.. important::

   During a long query executed in a session where a client (such as gsql, JDBC, or ODBC) directly connects to the CN,

   -  The CN checks the client connection status at the interval specified by **client_connection_check_interval**. If it detects that the client has been disconnected from the CN, the server terminates the long query and releases related resources to avoid waste of cluster resources.
   -  The DN checks its connection to the CN at the interval specified by **client_connection_check_interval**. If the DN detects that it has been disconnected from the CN, it terminates the long query and releases related resources to avoid waste of cluster resources.

conn_recycle_timeout
--------------------

**Parameter description**: the interval for reclaiming idle connections between a CN and other nodes to the connection pool. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 3600, in second (s). **0** indicates that the function of reclaiming idle connections is disabled.

**Default value**: **30**
