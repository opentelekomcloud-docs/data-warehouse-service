:original_name: dws_03_0025.html

.. _dws_03_0025:

Why Was I Not Notified of Failure Unbinding the EIP When GaussDB(DWS) Is Connected Over the Internet?
=====================================================================================================

After the EIP is unbound, the network may be disconnected. However, the TCP layer does not detect a faulty physical connection in time due to keepalive settings. As a result, the gsql, ODBC, and JDBC clients also cannot identify the network fault in time.

The duration when the database sends the disconnection message to the client depends on the keepalive settings. The specific algorithm for calculating the duration is:

**keepalive_time** + **keepalive_probes** x **keepalive_intvl**

Keepalive values affect network communication stability. Adjust them to service pressure and network conditions.

On Linux, run the **sysctl** command to modify the following parameters:

-  net.ipv4.tcp_keepalive_time
-  net.ipv4.tcp_keeaplive_probes
-  net.ipv4.tcp_keepalive_intvl

For example, if you want to change the value of **net.ipv4.tcp_keepalive_time**, run the following command to change it to **120**.

**sysctl net.ipv4.tcp_keepalive_time=120**

On Windows, modify the following configuration information in registry **HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\services\\Tcpip\\Parameters**:

-  KeepAliveTime
-  KeepAliveInterval
-  TcpMaxDataRetransmissions (equivalent to **tcp_keepalive_probes**)

.. note::

   If you cannot find the preceding parameters in registry **HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\services\\Tcpip\\Parameters**, add these parameters. Open **Registry Editor**, right-click the blank area on the right, and choose **Create** > **DWORD (32-bit) Value** to add these parameters.
