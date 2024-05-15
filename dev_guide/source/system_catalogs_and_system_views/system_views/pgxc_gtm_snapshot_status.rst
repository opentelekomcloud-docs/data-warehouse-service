:original_name: dws_04_0806.html

.. _dws_04_0806:

PGXC_GTM_SNAPSHOT_STATUS
========================

**PGXC_GTM_SNAPSHOT_STATUS** displays transaction information on the current GTM.

.. table:: **Table 1** PGXC_GTM_SNAPSHOT_STATUS columns

   +--------------+---------+----------------------------------------------------------------------------+
   | Name         | Type    | Description                                                                |
   +==============+=========+============================================================================+
   | xmin         | xid     | Minimum ID of the running transactions                                     |
   +--------------+---------+----------------------------------------------------------------------------+
   | xmax         | xid     | ID of the transaction next to the executed transaction with the maximum ID |
   +--------------+---------+----------------------------------------------------------------------------+
   | csn          | integer | Sequence number of the transaction to be committed                         |
   +--------------+---------+----------------------------------------------------------------------------+
   | oldestxmin   | xid     | Minimum ID of the executed transactions                                    |
   +--------------+---------+----------------------------------------------------------------------------+
   | xcnt         | integer | Number of the running transactions                                         |
   +--------------+---------+----------------------------------------------------------------------------+
   | running_xids | text    | IDs of the running transactions                                            |
   +--------------+---------+----------------------------------------------------------------------------+
