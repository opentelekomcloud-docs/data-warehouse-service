:original_name: dws_04_0891.html

.. _dws_04_0891:

Communication Library Parameters
================================

This section describes parameter settings and value ranges for communication libraries.

.. _en-us_topic_0000001188323630__s0011acc521b848fe85b25a68f534dc73:

comm_max_datanode
-----------------

**Parameter description**: Specifies the maximum number of DNs supported by the TCP proxy communication library or SCTP communication library.

**Type**: USERSET

**Value range**: an integer ranging from 1 to 8192

**Default value**: actual number of DNs

.. important::

   If the number of DNs is increased, the change takes effect immediately. If the number of DNs is reduced, the cluster needs to be restarted for the change to take effect.

comm_max_stream
---------------

**Parameter description**: Specifies the maximum number of concurrent data streams supported by the TCP proxy communication library or SCTP communication library. The value of this parameter must be greater than: Number of concurrent data streams x Number of operators in each stream x Square of SMP.

**Type**: POSTMASTER

**Value range**: an integer ranging from 1 to 60000

**Default value**: calculated by the following formula: min (query_dop_limit x query_dop_limit x 2 x 20, max_process_memory (bytes) x 0.025/(Maximum number of CNs + Number of current DNs)/260. If the value is less than 1024, 1024 is used. query_dop_limit = Number of CPU cores of a single server/Number of DNs of a single server.

.. note::

   -  You are not advised to set this parameter to a large value because this will cause high memory usage (256 bytes x **comm_max_stream** x **comm_max_datanode**). If the number of concurrent data streams is large, the query is complex and the smp is large, resulting in insufficient memory.
   -  If the value of :ref:`comm_max_datanode <en-us_topic_0000001188323630__s0011acc521b848fe85b25a68f534dc73>` is small, the process memory is sufficient. In this case, you can increase the value of **comm_max_stream**.

max_stream_pool
---------------

**Parameter description**: Specifies the maximum number of stream threads that can be contained in a stream thread pool. This feature is supported in 8.1.2 or later.

**Type**: SUSET

**Value range**: an integer ranging from -1 to INT_MAX. The values **-1** and **0** indicate that the stream thread pool is disabled.

**Default value**: **65535**

.. note::

   -  The number of stream threads in a thread pool can be reduced in real time. If the value of this parameter is increased, the number of stream threads is increased to meet the service requirements.
   -  Generally, you are advised not to change the value of this parameter because the stream thread pool supports the automatic cleanup function.
   -  If too many idle stream threads occupy the memory, you can decrease the value of this parameter to save the memory.

comm_max_receiver
-----------------

**Parameter description**: Specifies the maximum number of receiving threads for the TCP proxy communication library or SCTP communication library.

**Type**: POSTMASTER

**Value range**: an integer ranging from 1 to 50

**Default value**: 4

comm_quota_size
---------------

**Parameter description**: Specifies the maximum size of packets that can be consecutively sent by the TCP proxy communication library or SCTP communication library. When you use a 1GE NIC, a small value ranging from 20 KB to 40 KB is recommended.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 102400. The default unit is KB. The value **0** indicates that the quota mechanism is not used.

**Default value**: **1 MB**

comm_usable_memory
------------------

**Parameter description**: Specifies the maximum memory available for buffering on the TCP proxy communication library or SCTP communication library on a single DN.

**Type**: POSTMASTER

**Value range**: an integer ranging from 1 to 256. The default unit is KB. The minimum size cannot be less than 1 GB for installation.

**Default value**: **max_process_memory/8**

.. important::

   This parameter must be specifically set based on environment memory and the deployment method. If it is too large, there may be out-of-memory (OOM). If it is too small, the performance of the TCP proxy communication library or SCTP communication library may deteriorate.

comm_memory_pool_percent
------------------------

**Parameter description**: Specifies the percentage of the memory pool resources that can be used by the TCP proxy communication library or the SCTP communication library in a DN. This parameter is used to adaptively reserve memory used by the communication libraries.

**Type**: POSTMASTER

**Value range**: an integer ranging from 0 to 100

**Default value**: **0**

.. important::

   If the memory used by the communication library is small, set this parameter to a small value. Otherwise, set it to a large value.

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

**Parameter description**: Specifies the debug mode of the TCP proxy communication library or SCTP communication library, that is, whether to print logs about the communication layer. The setting is effective at the session layer.

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

**Parameter description**: Specifies the timer mode of the TCP proxy communication library or SCTP communication library, that is, whether to print timer logs in each phase of the communication layer. The setting is effective at the session layer.

.. important::

   When the switch is set to **on**, the number of printed logs is huge, adding extra overhead and reducing database performance. Therefore, set the switch to **on** only in the debug mode.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the detailed timer log of the communication library is printed.
-  **off** indicates the detailed timer log of the communication library is not printed.

**Default value**: **off**

comm_stat_mode
--------------

**Parameter description**: Specifies the statistics mode of the TCP proxy communication library or SCTP communication library, that is, whether to print statistics about the communication layer. The setting is effective at the session layer.

.. important::

   When the switch is set to **on**, the number of printed logs is huge, adding extra overhead and reducing database performance. Therefore, set the switch to **on** only in the debug mode.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the statistics log of the communication library is printed.
-  **off** indicates the statistics log of the communication library is not printed.

**Default value:** **off**

enable_stateless_pooler_reuse
-----------------------------

**Parameter description**: Specifies whether to enable the pooler reuse mode. The setting takes effect after the cluster is restarted.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** or **true** indicates that the pooler reuse mode is enabled.
-  **off** or **false** indicates that the pooler reuse mode is disabled.

   .. important::

      Set this parameter to the same value for CNs and DNs. If **enable_stateless_pooler_reuse** is set to **off** for CNs and set to **on** for DNs, the cluster communication fails. Restart the cluster to make the setting take effect.

**Default value**: **off**

comm_cn_dn_logic_conn
---------------------

**Parameter description**: Specifies a switch for logical connections between CNs and DNs. The parameter setting takes effect only after the cluster is restarted.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** or **true** indicates that the connections between CNs and DNs are logical, with the libcomm component in use.
-  **off** or **false** indicates that the connections between CNs and DNs are physical, with the libpq component in use.

   .. important::

      If **comm_cn_dn_logic_conn** is set to **off** for CNs and set to **on** for DNs, cluster communication will fail. You are advised to set this parameter to the same value for all CNs and DNs. Restart the cluster to make the setting take effect.

**Default value:** **off**
