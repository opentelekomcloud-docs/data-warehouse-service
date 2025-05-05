:original_name: dws_04_1077.html

.. _dws_04_1077:

PGXC_POOLER_STATUS
==================

PGXC_POOLER_STATUS displays the pooler cache connection status of each CN in the current cluster. This view can be queried only on CNs to display the connection cache information of the pooler module on all CNs. The **PGXC_POOLER_STATUS** view is supported only by clusters of version 8.3.0 or later.

.. table:: **Table 1** PGXC_POOLER_STATUS columns

   +-----------------------+-----------------------+--------------------------------------------------------------+
   | Column                | Type                  | Description                                                  |
   +=======================+=======================+==============================================================+
   | coorname              | Text                  | Name of the CN node.                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | database              | Text                  | Database name.                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | user_name             | Text                  | Username.                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | tid                   | Bigint                | ID of the thread used for the connection to the CN.          |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | node_oid              | Bigint                | OID of the node connected to.                                |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | node_name             | Name                  | Name of the node connected to.                               |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | in_use                | boolean               | Whether the connection is currently in use. The options are: |
   |                       |                       |                                                              |
   |                       |                       | -  **t** (true): The connection is in use.                   |
   |                       |                       | -  **f** (false): The connection is not in use.              |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | fdsock                | Bigint                | Peer socket.                                                 |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | remote_pid            | Bigint                | Peer thread ID.                                              |
   +-----------------------+-----------------------+--------------------------------------------------------------+
   | session_params        | Text                  | GUC session parameters issued by this connection.            |
   +-----------------------+-----------------------+--------------------------------------------------------------+
