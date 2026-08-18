:original_name: dws_04_0635.html

.. _dws_04_0635:

PGXC_NODE
=========

**PGXC_NODE** records information about cluster nodes.

.. table:: **Table 1** PGXC_NODE columns

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                                        |
   +=======================+=======================+====================================================================================================================================================+
   | node_name             | Name                  | Node name                                                                                                                                          |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | node_type             | Char                  | Node type                                                                                                                                          |
   |                       |                       |                                                                                                                                                    |
   |                       |                       | **C**: CN                                                                                                                                          |
   |                       |                       |                                                                                                                                                    |
   |                       |                       | **D**: DN                                                                                                                                          |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | node_port             | Integer               | Port ID of the node                                                                                                                                |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | node_host             | Name                  | Host name or IP address of a node. (If a virtual IP address is configured, its value is a virtual IP address.)                                     |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | node_port1            | Integer               | Port number of a replication node                                                                                                                  |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | node_host1            | Name                  | Host name or IP address of a replication node. (If a virtual IP address is configured, its value is a virtual IP address.)                         |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | hostis_primary        | boolean               | Whether a switchover occurs between the primary and the standby server on the current node                                                         |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | nodeis_primary        | boolean               | Whether the current node is preferred to execute non-query operations in the **replication** table                                                 |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | nodeis_preferred      | boolean               | Whether the current node is preferred to execute queries in the **replication** table                                                              |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | node_id               | Integer               | Node identifier                                                                                                                                    |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | sctp_port             | Integer               | Specifies the port used by the TCP proxy communication library or SCTP communication library of the primary node to listen to the data channel.    |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | control_port          | Integer               | Specifies the port used by the TCP proxy communication library or SCTP communication library of the primary node to listen to the control channel. |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | sctp_port1            | Integer               | Specifies the port used by the TCP proxy communication library or SCTP communication library of the standby node to listen to the data channel.    |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | control_port1         | Integer               | Specifies the port used by the TCP proxy communication library or SCTP communication library of the standby node to listen to the control channel. |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+
   | nodeis_central        | boolean               | Indicates that the current node is the central node.                                                                                               |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Query the CN and DN information of the cluster.

::

   SELECT * FROM pgxc_node;
     node_name   | node_type | node_port | node_host | node_port1 | node_host1 | hostis_primary | nodeis_primary | nodeis_preferred |   node_id
    | sctp_port | control_port | sctp_port1 | control_port1 | nodeis_central | read_only
   --------------+-----------+-----------+-----------+------------+------------+----------------+----------------+------------------+------------
   -+-----------+--------------+------------+---------------+----------------+-----------
    datanode1    | D         |     55504 | localhost |      55504 | localhost  | t              | f              | f                |   888802358
    |     55505 |        55507 |          0 |             0 | f              | f
    datanode2    | D         |     55508 | localhost |      55508 | localhost  | t              | f              | f                |  -905831925
    |     55509 |        55511 |          0 |             0 | f              | f
    coordinator1 | C         |     55500 | localhost |      55500 | localhost  | t              | f              | f                |  1938253334
    |         0 |            0 |          0 |             0 | t              | f
    datanode3    | D         |     55542 | localhost |      55542 | localhost  | t              | f              | f                | -1894792127
    |     57552 |        55544 |          0 |             0 | f              | t
    datanode4    | D         |     55546 | localhost |      55546 | localhost  | t              | f              | f                | -1307323892
    |     57808 |        55548 |          0 |             0 | f              | t
    datanode5    | D         |     55550 | localhost |      55550 | localhost  | t              | f              | f                |  1797586929
    |     58064 |        55552 |          0 |             0 | f              | t
    datanode6    | D         |     55554 | localhost |      55554 | localhost  | t              | f              | f                |   587455710
    |     58320 |        55556 |          0 |             0 | f              | t
    datanode7    | D         |     55558 | localhost |      55558 | localhost  | t              | f              | f                | -1685037427
    |     58576 |        55560 |          0 |             0 | f              | t
    datanode8    | D         |     55562 | localhost |      55562 | localhost  | t              | f              | f                |  -993847320
    |     58832 |        55564 |          0 |             0 | f              | t
   (9 rows)
