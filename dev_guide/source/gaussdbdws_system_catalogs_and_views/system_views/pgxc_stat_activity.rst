:original_name: dws_04_0820.html

.. _dws_04_0820:

PGXC_STAT_ACTIVITY
==================

**PGXC_STAT_ACTIVITY** displays information about the query performed by the current user on all the CNs in the current cluster.

.. table:: **Table 1** PGXC_STAT_ACTIVITY columns

   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name                  | Type                     | Description                                                                                                                                                                                                                                   |
   +=======================+==========================+===============================================================================================================================================================================================================================================+
   | coorname              | text                     | Name of the CN in the current cluster                                                                                                                                                                                                         |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | datid                 | oid                      | OID of the database that the user session connects to in the backend                                                                                                                                                                          |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | datname               | name                     | Name of the database that the user session connects to in the backend                                                                                                                                                                         |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pid                   | bigint                   | ID of the backend thread                                                                                                                                                                                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lwtid                 | integer                  | Lightweight thread ID of the backend thread                                                                                                                                                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | usesysid              | oid                      | OID of the user logging in to the backend                                                                                                                                                                                                     |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | usename               | name                     | Name of the user logging in to the backend                                                                                                                                                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | application_name      | text                     | Name of the application connected to the backend                                                                                                                                                                                              |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | client_addr           | inet                     | IP address of the client connected to the backend. If this column is **null**, it indicates either that the client is connected via a Unix socket on the server machine or that this is an internal process such as autovacuum.               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | client_hostname       | text                     | Host name of the connected client, as reported by a reverse DNS lookup of **client_addr**. This column will only be non-null for IP connections, and only when **log_hostname** is enabled.                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | client_port           | integer                  | TCP port number that the client uses for communication with this backend, or **-1** if a Unix socket is used                                                                                                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | backend_start         | timestamp with time zone | Startup time of the backend process, that is, the time when the client connects to the server                                                                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | xact_start            | timestamp with time zone | Time when the current transaction was started, or **NULL** if no transaction is active. If the current query is the first of its transaction, this column is equal to the **query_start** column.                                             |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query_start           | timestamp with time zone | Time when the currently active query was started, or time when the last query was started if **state** is not **active**                                                                                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | state_change          | timestamp with time zone | Time for the last status change                                                                                                                                                                                                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | waiting               | boolean                  | The value is **t** if the backend is waiting for a lock or node. Otherwise, the value is **f**.                                                                                                                                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enqueue               | text                     | Queuing status of a statement. Its value can be:                                                                                                                                                                                              |
   |                       |                          |                                                                                                                                                                                                                                               |
   |                       |                          | -  **waiting in global queue**: The statement is in the global concurrent queues.                                                                                                                                                             |
   |                       |                          | -  **waiting in respool queue**: The statement is queuing in the resource pool. The scenarios are as follows:                                                                                                                                 |
   |                       |                          |                                                                                                                                                                                                                                               |
   |                       |                          |    #. When dynamic load balancing is enabled, the number of simple jobs exceeds the upper limit (**max_dop**) of concurrent jobs on the fast lane.                                                                                            |
   |                       |                          |    #. When dynamic load balancing is disabled, the number of simple jobs exceeds the upper limit (**max_dop**) of concurrent jobs on the fast lane or the number of complex jobs exceeds the upper limit of concurrent jobs on the slow lane. |
   |                       |                          |                                                                                                                                                                                                                                               |
   |                       |                          | -  **waiting in ccn queue**: The job is in the CCN queue, which may be global memory queuing, slow lane memory queuing, or concurrent queuing.                                                                                                |
   |                       |                          | -  Empty or **no waiting queue**: The statement is running.                                                                                                                                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | state                 | text                     | Overall state of the backend. Its value can be:                                                                                                                                                                                               |
   |                       |                          |                                                                                                                                                                                                                                               |
   |                       |                          | -  **active**: The backend is executing a query.                                                                                                                                                                                              |
   |                       |                          | -  **idle**: The backend is waiting for a new client command.                                                                                                                                                                                 |
   |                       |                          | -  **idle in transaction**: The backend is in a transaction, but there is no statement being executed in the transaction.                                                                                                                     |
   |                       |                          | -  **idle in transaction (aborted)**: The backend is in a transaction, but there are statements failed in the transaction.                                                                                                                    |
   |                       |                          | -  **fastpath function call**: The backend is executing a **fast-path** function.                                                                                                                                                             |
   |                       |                          | -  **disabled**: This state is reported if **track_activities** is disabled in this backend.                                                                                                                                                  |
   |                       |                          |                                                                                                                                                                                                                                               |
   |                       |                          | .. note::                                                                                                                                                                                                                                     |
   |                       |                          |                                                                                                                                                                                                                                               |
   |                       |                          |    Only system administrators can view the session status of their accounts. The state information of other accounts is empty.                                                                                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_pool         | name                     | Resource pool used by the user                                                                                                                                                                                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | stmt_type             | text                     | Type of a user statement                                                                                                                                                                                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query_id              | bigint                   | ID of a query                                                                                                                                                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query                 | text                     | Text of this backend's most recent query If the **state** is **active**, this column shows the executing query. In all other states, it shows the last query that was executed.                                                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connection_info       | text                     | A string in JSON format recording the driver type, driver version, driver deployment path, and process owner of the connected database (for details, see :ref:`connection_info <en-us_topic_0000001460722472__section4834457114318>`).        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Run the following command to view blocked query statements.

::

   SELECT datname,usename,state,query FROM PGXC_STAT_ACTIVITY WHERE waiting = true;

Check the working status of the snapshot thread.

::

   SELECT application_name,backend_start,state_change,state,query FROM PGXC_STAT_ACTIVITY WHERE application_name='WDRSnapshot';

View the running query statements.

::

   SELECT datname,usename,state,pid FROM PGXC_STAT_ACTIVITY;
    datname  | usename | state  |       pid
   ----------+---------+--------+-----------------
    gaussdb | Ruby    | active | 140298793514752
    gaussdb | Ruby    | active | 140298718004992
    gaussdb | Ruby    | idle   | 140298650908416
    gaussdb | Ruby    | idle   | 140298625742592
    gaussdb | dbadmin | active | 140298575406848
   (5 rows)

View the number of session connections that have been used by postgres. **1** indicates the number of session connections that have been used by **postgres**.

::

   SELECT COUNT(*) FROM PGXC_STAT_ACTIVITY WHERE DATNAME='postgres';
    count
   -------
        1
   (1 row)
