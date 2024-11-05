:original_name: dws_04_1077.html

.. _dws_04_1077:

PGXC_POOLER_STATUS
==================

PGXC_POOLER_STATUS displays the pooler cache connection status of each CN in the current cluster. This view can be queried only on CNs to display the connection cache information of the pooler module on all CNs. The **PGXC_POOLER_STATUS** view is supported only by clusters of versions 8.2.1.300 or later.

.. table:: **Table 1** PGXC_POOLER_STATUS columns

   +-----------------------+-----------------------+----------------------------------------------------------------+
   | Column                | Type                  | Description                                                    |
   +=======================+=======================+================================================================+
   | coorname              | text                  | CN node name                                                   |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | database              | text                  | Database name                                                  |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | user_name             | text                  | Username                                                       |
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
   | fdsock                | bigint                | Peer socket                                                    |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | remote_pid            | bigint                | Peer thread ID                                                 |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | session_params        | text                  | GUC session parameter delivered by the connection              |
   +-----------------------+-----------------------+----------------------------------------------------------------+
