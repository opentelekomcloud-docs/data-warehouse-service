:original_name: dws_04_0806.html

.. _dws_04_0806:

PGXC_GTM_SNAPSHOT_STATUS
========================

**PGXC_GTM_SNAPSHOT_STATUS** displays transaction information on the current GTM.

.. table:: **Table 1** PGXC_GTM_SNAPSHOT_STATUS columns

   +--------------+---------+----------------------------------------------------------------------------+
   | Column       | Type    | Description                                                                |
   +==============+=========+============================================================================+
   | xmin         | Xid     | Minimum ID of the running transactions                                     |
   +--------------+---------+----------------------------------------------------------------------------+
   | xmax         | Xid     | ID of the transaction next to the executed transaction with the maximum ID |
   +--------------+---------+----------------------------------------------------------------------------+
   | csn          | Integer | Sequence number of the transaction to be committed                         |
   +--------------+---------+----------------------------------------------------------------------------+
   | oldestxmin   | Xid     | Minimum ID of the executed transactions                                    |
   +--------------+---------+----------------------------------------------------------------------------+
   | xcnt         | Integer | Number of the running transactions                                         |
   +--------------+---------+----------------------------------------------------------------------------+
   | running_xids | Text    | IDs of the running transactions                                            |
   +--------------+---------+----------------------------------------------------------------------------+
