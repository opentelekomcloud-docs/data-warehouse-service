:original_name: dws_04_0890.html

.. _dws_04_0890:

Security and Authentication (postgresql.conf)
=============================================

This section describes parameters about how to securely authenticate the client and server.

session_timeout
---------------

**Parameter description**: Specifies the maximum idle time without any operations after a connection to the server is established.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 86400. The minimum unit is second (s). **0** means to disable the timeout.

**Default value**: **10 min**

.. important::

   -  The gsql client of DWS has an automatic reconnection mechanism. If the initialized local connection of a user to the server times out, gsql disconnects from and reconnects to the server.
   -  Connections from the pooler connection pool to other CNs and DNs are not controlled by the **session_timeout** parameter.

ssl_renegotiation_limit
-----------------------

**Parameter description**: Specifies the traffic volume over the SSL-encrypted channel before the session key is renegotiated. The renegotiation traffic limitation mechanism reduces the probability that attackers use the password analysis method to crack the key based on a huge amount of data but causes big performance losses. The traffic indicates the sum of sent and received traffic.

**Type**: USERSET

.. note::

   You are advised to retain the default value, that is, disable the renegotiation mechanism. You are not advised to use the **gs_guc** tool or other methods to set the **ssl_renegotiation_limit** parameter in the **postgresql.conf** file. The setting does not take effect.

**Value range**: an integer ranging from 0 to **INT_MAX**. The unit is KB. **0** indicates that the renegotiation mechanism is disabled.

**Default value**: **0**

failed_login_attempts
---------------------

**Parameter description**: Specifies the maximum number of consecutive login failures. If the number of incorrect password attempts reaches the value of this parameter, the account is locked and will be automatically unlocked after the value of **password_lock_time** (unit: seconds) is reached.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 1000

-  **0** indicates that the automatic locking function does not take effect.
-  A positive number indicates that an account is locked when the number of incorrect password attempts reaches the value of **failed_login_attempts**.

**Default value**: **10**

.. important::

   -  **failed_login_attempts** and **password_lock_time** must be set to positive numbers, or they will not take effect. (The default value of **password_lock_time** is one day. To set it, contact technical support.)
   -  **failed_login_attempts** works with the SSL connection mode of the client to identify the number of incorrect password attempts. If PGSSLMODE is set to **allow** or **prefer**, two connection requests are generated for a password connection request. One request attempts an SSL connection, and the other request attempts a non-SSL connection. In this case, the number of incorrect password attempts perceived by the user is the value of **failed_login_attempts** divided by 2.
