:original_name: dws_04_0740.html

.. _dws_04_0740:

PG_POOLER_STATUS
================

**PG_POOLER_STATUS** displays the cache connection status in the pooler. **PG_POOLER_STATUS** can only query on the CN, and displays the connection cache information about the pooler module.

.. table:: **Table 1** PG_POOLER_STATUS columns

   +-----------------------+-----------------------+----------------------------------------------------------------+
   | Name                  | Type                  | Description                                                    |
   +=======================+=======================+================================================================+
   | database              | text                  | Database name                                                  |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | user_name             | text                  | User name                                                      |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | tid                   | bigint                | ID of a thread connected to the CN                             |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | node_oid              | bigint                | OID of the node connected                                      |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | node_name             | name                  | Name of the node connected                                     |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | in_use                | boolean               | Whether the connection is in use                               |
   |                       |                       |                                                                |
   |                       |                       | -  **t** (true): indicates that the connection is in use.      |
   |                       |                       | -  **f** (false): indicates that the connection is not in use. |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | fdsock                | bigint                | Peer socket.                                                   |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | remote_pid            | bigint                | Peer thread ID.                                                |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | session_params        | text                  | GUC session parameter delivered by the connection.             |
   +-----------------------+-----------------------+----------------------------------------------------------------+

Example
-------

View information about the connection pool **pooler**:

::

   select database,user_name,node_name,in_use,count(*) from pg_pooler_status group by 1, 2, 3 ,4 order by 5 desc limit 50;
    database | user_name |  node_name   | in_use | count
   ----------+-----------+--------------+--------+-------
    mydbdemo | user3     | cn_5001      | f      |     2
    mydbdemo | user3     | dn_6005_6006 | t      |     2
    mydbdemo | user3     | dn_6001_6002 | t      |     2
    mydbdemo | user3     | dn_6003_6004 | f      |     2
    mydbdemo | user3     | dn_6003_6004 | t      |     2
    mydbdemo | user3     | dn_6005_6006 | f      |     2
    mydbdemo | user3     | dn_6001_6002 | f      |     2
    mydbdemo | user3     | cn_5002      | f      |     2
    gaussdb  | user3     | dn_6003_6004 | f      |     1
    mydbdemo | user3     | cn_5001      | t      |     1
    music    | user2     | dn_6003_6004 | f      |     1
    music    | user2     | dn_6005_6006 | f      |     1
    gaussdb  | user1     | dn_6005_6006 | f      |     1
   (13 rows)
