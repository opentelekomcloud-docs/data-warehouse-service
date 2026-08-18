:original_name: dws_04_0810.html

.. _dws_04_0810:

PGXC_NODE_ENV
=============

**PGXC_NODE_ENV** displays the environmental variables information about all nodes in a cluster.

.. table:: **Table 1** PGXC_NODE_ENV columns

   +---------------+---------+-----------------------------------------------------+
   | Column        | Type    | Description                                         |
   +===============+=========+=====================================================+
   | node_name     | Text    | Names of all nodes in the cluster.                  |
   +---------------+---------+-----------------------------------------------------+
   | host          | Text    | Host names of all nodes in the cluster.             |
   +---------------+---------+-----------------------------------------------------+
   | process       | Integer | Process IDs of all nodes in the cluster.            |
   +---------------+---------+-----------------------------------------------------+
   | port          | Integer | Port numbers of all nodes in the cluster.           |
   +---------------+---------+-----------------------------------------------------+
   | installpath   | Text    | Installation directory of all nodes in the cluster. |
   +---------------+---------+-----------------------------------------------------+
   | datapath      | Text    | Data directories of all nodes in the cluster.       |
   +---------------+---------+-----------------------------------------------------+
   | log_directory | Text    | Log directories of all nodes in the cluster.        |
   +---------------+---------+-----------------------------------------------------+
