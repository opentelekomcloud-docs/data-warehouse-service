:original_name: dws_03_0025.html

.. _dws_03_0025:

Why Was I Not Notified of Failure After Unbinding the EIP When GaussDB(DWS) Is Connected Over the Internet?
===========================================================================================================

The network is disconnected when the EIP is unbound. However, the TCP layer does not detect a faulty physical connection in time due to keepalive settings. As a result, the gsql, ODBC, and JDBC clients also cannot identify the network fault in time.

The time for the client to wait for the database to return is related to the setting of the **keepalive** parameter, and may be specifically expressed as: **keepalive_time** + **keepalive_probes**\ \*\ **keepalive_intvl**.

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
