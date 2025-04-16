:original_name: dws_gsql_008.html

.. _dws_gsql_008:

.. _en-us_topic_0000001860198837:

Troubleshooting
===============

Low Connection Performance
--------------------------

-  The database kernel slowly runs the initialization statement.

   Problems are difficult to locate in this scenario. Try using the **strace** Linux trace command.

   .. code-block::

      strace gsql -U MyUserName -W {password} -d postgres -h 127.0.0.1 -p 23508 -r -c '\q'

   The database connection process will be printed on the screen. If the following statement takes a long time to run:

   .. code-block::

      sendto(3, "Q\0\0\0\25SELECT VERSION()\0", 22, MSG_NOSIGNAL, NULL, 0) = 22
      poll([{fd=3, events=POLLIN|POLLERR}], 1, -1) = 1 ([{fd=3, revents=POLLIN}])

   It indicates that **SELECT VERSION()** statement was run slowly.

   After the database is connected, you can run the **explain performance select version()** statement to find the reason why the initialization statement was run slowly. For details, see "Introduction to the SQL Execution Plan" in the *Developer Guide*.

   An uncommon scenario is that the disk of the machine where the CN resides is full or faulty, affecting queries and leading to user authentication failures. As a result, the connection process is suspended. To solve this problem, simply clear the data disk space of the CN.

-  TCP connection is set up slowly.

   Adapt the steps of troubleshooting slow initialization statement execution. Use **strace**. If the following statement was run slowly:

   .. code-block::

      connect(3, {sa_family=AF_FILE, path="/home/test/tmp/gaussdb_llt1/.s.PGSQL.61052"}, 110) = 0

   Or

   .. code-block::

      connect(3, {sa_family=AF_INET, sin_port=htons(61052), sin_addr=inet_addr("127.0.0.1")}, 16) = -1 EINPROGRESS (Operation now in progress)

   It indicates that the physical connection between the client and the database was set up slowly. In this case, check whether the network is unstable or has high throughput.

Problems in Setting Up Connections
----------------------------------

-  gsql: could not connect to server: No route to host

   This problem occurs generally because an unreachable IP address or port number was specified. Check whether the values of **-h** and **-p** parameters are correct.

-  gsql: FATAL: Invalid username/password,login denied.

   This problem occurs generally because an incorrect user name or password was entered. Contact the database administrator to check whether the user name and password are correct.

-  The "libpq.so" loaded mismatch the version of gsql, please check it.

   This problem occurs because the version of **libpq.so** used in the environment does not match that of **gsql**. Run the **ldd gsql** command to check the version of the loaded **libpq.so**, and then load correct **libpq.so** by modifying the environment variable **LD_LIBRARY_PATH**.

-  gsql: symbol lookup error: xxx/gsql: undefined symbol: libpqVersionString

   This problem occurs because the version of **libpq.so** used in the environment does not match that of **gsql** (or the PostgreSQL **libpq.so** exists in the environment). Run the **ldd gsql** command to check the version of the loaded **libpq.so**, and then load correct **libpq.so** by modifying the environment variable **LD_LIBRARY_PATH**.

-  gsql: connect to server failed: Connection timed out

   Is the server running on host "xx.xxx.xxx.xxx" and accepting TCP/IP connections on port xxxx?

   This problem is caused by network connection faults. Check the network connection between the client and the database server. If you cannot ping from the client to the database server, the network connection is abnormal. Contact network management personnel for troubleshooting.

   .. code-block::

      ping -c 4 10.10.10.1
      PING 10.10.10.1 (10.10.10.1) 56(84) bytes of data.
      From 10.10.10.1: icmp_seq=2 Destination Host Unreachable
      From 10.10.10.1 icmp_seq=2 Destination Host Unreachable
      From 10.10.10.1 icmp_seq=3 Destination Host Unreachable
      From 10.10.10.1 icmp_seq=4 Destination Host Unreachable
      --- 10.10.10.1 ping statistics ---
      4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 2999ms

-  gsql: FATAL: permission denied for database "postgres"

   DETAIL: User does not have CONNECT privilege.

   This problem occurs because the user does not have the permission to access the database. To solve this problem, perform the following steps:

   #. Connect to the database as the system administrator **dbadmin**.

      .. code-block::

         gsql -d postgres -U dbadmin -p 8000

   #. Grant the users with the access permission to the database.

      GRANT CONNECT ON DATABASE postgres TO user1;

      .. note::

         Common misoperations may also cause a database connection failure, for example, entering an incorrect database name, user name, or password. In this case, the client tool will display the corresponding error messages.

         .. code-block::

            gsql -d postgres -p 8000
            gsql: FATAL:  database "postgres" does not exist

            gsql -d postgres -U user1 -W gauss@789 -p 8000
            gsql: FATAL:  Invalid username/password,login denied.

-  gsql: FATAL: sorry, too many clients already, active/non-active: 197/3.

   This problem occurs because the number of system connections exceeds the allowed maximum. Contact the database administrator to release unnecessary sessions.

   You can check the number of connections as described in :ref:`Table 1 <en-us_topic_0000001860198837__en-us_topic_0000001813439088_en-us_topic_0000001145650331_tee9a019644dc4b32aad8d0aa00a7e061>`.

   You can view the session status in the **PG_STAT_ACTIVITY** view. To release unnecessary sessions, use the pg_terminate_backend function.

   .. code-block::

      select datid,pid,state from pg_stat_activity;

   .. code-block::

       datid |       pid       | state
      -------+-----------------+--------
       13205 | 139834762094352 | active
       13205 | 139834759993104 | idle
      (2 rows)

   The value of pid is the thread ID of the session. Terminate the session using its thread ID.

   .. code-block::

      SELECT PG_TERMINATE_BACKEND(139834759993104);

   If information similar to the following is displayed, the session is successfully terminated:

   .. code-block::

      PG_TERMINATE_BACKEND
      ----------------------
       t
      (1 row)

   .. _en-us_topic_0000001860198837__en-us_topic_0000001813439088_en-us_topic_0000001145650331_tee9a019644dc4b32aad8d0aa00a7e061:

   .. table:: **Table 1** Viewing the numbers of connections

      +--------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Description                                                        | Command                                                                                                                                                                                               |
      +====================================================================+=======================================================================================================================================================================================================+
      | View the upper limit of a user's connections.                      | Run the following command to view the upper limit of user **USER1**'s connections. **-1** indicates that no connection upper limit is set for user **USER1**.                                         |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    | .. code-block::                                                                                                                                                                                       |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |    SELECT ROLNAME,ROLCONNLIMIT FROM PG_ROLES WHERE ROLNAME='user1';                                                                                                                                   |
      |                                                                    |     rolname | rolconnlimit                                                                                                                                                                            |
      |                                                                    |    ---------+--------------                                                                                                                                                                           |
      |                                                                    |     user1    |           -1                                                                                                                                                                           |
      |                                                                    |    (1 row)                                                                                                                                                                                            |
      +--------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of connections that have been used by a user.      | Run the following command to view the number of connections that have been used by user **user1**. **1** indicates the number of connections that have been used by user **user1**.                   |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    | .. code-block::                                                                                                                                                                                       |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |    SELECT COUNT(*) FROM V$SESSION WHERE USERNAME='user1';                                                                                                                                             |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |     count                                                                                                                                                                                             |
      |                                                                    |    -------                                                                                                                                                                                            |
      |                                                                    |         1                                                                                                                                                                                             |
      |                                                                    |    (1 row)                                                                                                                                                                                            |
      +--------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the upper limit of connections to database.                   | Run the following command to view the upper limit of connections used by **postgres**. **-1** indicates that no upper limit is set for the number of connections that have been used by **postgres**. |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    | .. code-block::                                                                                                                                                                                       |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |    SELECT DATNAME,DATCONNLIMIT FROM PG_DATABASE WHERE DATNAME='postgres';                                                                                                                             |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |     datname  | datconnlimit                                                                                                                                                                           |
      |                                                                    |    ----------+--------------                                                                                                                                                                          |
      |                                                                    |     postgres |           -1                                                                                                                                                                           |
      |                                                                    |    (1 row)                                                                                                                                                                                            |
      +--------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of connections that have been used by a database.  | Run the following command to view the number of connections that have been used by **postgres**. **1** indicates the number of connections that have been used by **postgres**.                       |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    | .. code-block::                                                                                                                                                                                       |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |    SELECT COUNT(*) FROM PG_STAT_ACTIVITY WHERE DATNAME='postgres';                                                                                                                                    |
      |                                                                    |     count                                                                                                                                                                                             |
      |                                                                    |    -------                                                                                                                                                                                            |
      |                                                                    |         1                                                                                                                                                                                             |
      |                                                                    |    (1 row)                                                                                                                                                                                            |
      +--------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the total number of connections that have been used by users. | Run the following command to view the number of connections that have been used by users:                                                                                                             |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    | .. code-block::                                                                                                                                                                                       |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |    SELECT COUNT(*) FROM V$SESSION;                                                                                                                                                                    |
      |                                                                    |                                                                                                                                                                                                       |
      |                                                                    |     count                                                                                                                                                                                             |
      |                                                                    |    -------                                                                                                                                                                                            |
      |                                                                    |         10                                                                                                                                                                                            |
      |                                                                    |    (1 row)                                                                                                                                                                                            |
      +--------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  gsql: wait xxx.xxx.xxx.xxx:xxxx timeout expired

   When **gsql** initiates a connection request to the database, a 5-minute timeout period is used. If the database cannot correctly authenticate the client request and client identity within this period, **gsql** will exit the connection process for the current session, and will report the above error.

   Generally, this problem is caused by the incorrect host and port (that is, the *xxx* part in the error information) specified by the **-h** and **-p** parameters. As a result, the communication fails. Occasionally, this problem is caused by network faults. To resolve this problem, check whether the host name and port number of the database are correct.

-  gsql: could not receive data from server: Connection reset by peer.

   Check whether CN logs contain information similar to "FATAL: cipher file "/data/coordinator/server.key.cipher" has group or world access". This error is usually caused by incorrect tampering with the permissions for data directories or some key files. For details about how to correct the permissions, see related permissions for files on other normal instances.

-  gsql: FATAL: GSS authentication method is not allowed because XXXX user password is not disabled.

   In **pg_hba.conf** of the target CN, the authentication mode is set to **gss** for authenticating the IP address of the current client. However, this authentication algorithm cannot authenticate clients. Change the authentication algorithm to **sha256** and try again.

   .. note::

      -  Do not modify the configurations of database cluster hosts in the **pg_hba. conf** file. Otherwise, the database may become faulty.
      -  You are advised to deploy service applications outside the database cluster.

Other Faults
------------

-  There is a core dump or abnormal exit due to the bus error.

   Generally, this problem is caused by changes in loading the shared dynamic library (.so file in Linux) during process running. Alternatively, if the process binary file changes, the execution code for the OS to load machines or the entry for loading a dependent library will change accordingly. In this case, the OS kills the process for protection purposes, generating a core dump file.

   To resolve this problem, try again. In addition, do not run service programs in a cluster during O&M operations, such as an upgrade, preventing such a problem caused by file replacement during the upgrade.

   .. note::

      A possible stack of the core dump file contains dl_main and its function calling. The file is used by the OS to initialize a process and load the shared dynamic library. If the process has been initialized but the shared dynamic library has not been loaded, the process cannot be considered completely started.
