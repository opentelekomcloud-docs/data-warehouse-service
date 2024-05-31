:original_name: dws_06_0344.html

.. _dws_06_0344:

Transaction IDs and Snapshots
=============================

The following functions provide server transaction information in an exportable form. The main use of these functions is to determine which transactions were committed between two snapshots.

pgxc_is_committed(transaction_id)
---------------------------------

Description: Determines whether the given XID is committed or ignored. NULL indicates the unknown status (such as running, preparing, and freezing).

Return type: Boolean

txid_current()
--------------

Description: Gets current transaction ID.

Return type: bigint

txid_current_snapshot()
-----------------------

Description: Gets current snapshot.

Return type: txid_snapshot

txid_snapshot_xip(txid_snapshot)
--------------------------------

Description: Gets in-progress transaction IDs in snapshot.

Return type: setof bigint

txid_snapshot_xmax(txid_snapshot)
---------------------------------

Description: Gets **xmax** of snapshot.

Return type: bigint

txid_snapshot_xmin(txid_snapshot)
---------------------------------

Description: Gets **xmin** of snapshot.

Return type: bigint

txid_visible_in_snapshot(bigint, txid_snapshot)
-----------------------------------------------

Description: Queries whether the transaction ID is visible in snapshot. (do not use with subtransaction IDs)

Return type: boolean

The internal transaction ID type (**xid**) is 32 bits wide and wraps around every 4 billion transactions. **txid_snapshot**, the data type used by these functions, stores information about transaction ID visibility at a particular moment in time. :ref:`Table 1 <en-us_topic_0000001446551144__table968413271422>` describes its components.

.. _en-us_topic_0000001446551144__table968413271422:

.. table:: **Table 1** Snapshot components

   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name     | Description                                                                                                                                                                                                                                                                                                                                                                                           |
   +==========+=======================================================================================================================================================================================================================================================================================================================================================================================================+
   | xmin     | Earliest transaction ID (txid) that is still active. All earlier transactions will either be committed and visible, or rolled back.                                                                                                                                                                                                                                                                   |
   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | xmax     | First as-yet-unassigned txid. All txids greater than or equal to this are not yet started as of the time of the snapshot, so they are invisible.                                                                                                                                                                                                                                                      |
   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | xip_list | Active txids at the time of the snapshot. The list includes only those active txids between **xmin** and **xmax**; there might be active txids higher than **xmax**. A txid that is **xmin <= txid < xmax** and not in this list was already completed at the time of the snapshot, and is either visible or dead according to its commit status. The list does not include txids of subtransactions. |
   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**txid_snapshot**'s textual representation is **xmin:xmax:xip_list**.

For example: **10:20:10,14,15** means **xmin=10, xmax=20, xip_list=10, 14, 15**.
