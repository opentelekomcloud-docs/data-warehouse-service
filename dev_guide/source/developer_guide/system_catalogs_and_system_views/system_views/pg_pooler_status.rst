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
