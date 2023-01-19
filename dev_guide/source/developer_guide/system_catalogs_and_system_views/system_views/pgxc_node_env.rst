:original_name: dws_04_0810.html

.. _dws_04_0810:

PGXC_NODE_ENV
=============

**PGXC_NODE_ENV** displays the environmental variables information about all nodes in a cluster.

.. table:: **Table 1** PGXC_NODE_ENV columns

   ============= ======= ==================================================
   Name          Type    Description
   ============= ======= ==================================================
   node_name     text    Names of all nodes in the cluster
   host          text    Host names of all nodes in the cluster
   process       integer Process IDs of all nodes in the cluster
   port          integer Port numbers of all nodes in the cluster
   installpath   text    Installation directory of all nodes in the cluster
   datapath      text    Data directory of all nodes in the cluster
   log_directory text    Log directory of all nodes in the cluster
   ============= ======= ==================================================
