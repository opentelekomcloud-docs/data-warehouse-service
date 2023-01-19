:original_name: dws_06_0061.html

.. _dws_06_0061:

Replication Functions
=====================

A replication function synchronizes logs and data between instances. It is a statistics or operation method provided by the system to implement HA.

.. note::

   Replication functions except statistics queries are internal functions. You are not advised to use them directly.

-  pg_create_logical_replication_slot('slot_name', 'plugin_name')

   Description: Creates a logical replication slot.

   Parameter:

   -  slot_name

      Indicates the name of the streaming replication slot.

      Value range: a string, supporting only letters, digits, and the following special characters: \_?-.

   -  plugin_name

      Indicates the name of the plugin.

      Value range: a string, supporting only **mppdb_decoding**

   Return type: name, text

   Note: The first return value is the slot name, and the second is the start LSN position for decoding in the logical replication slot.

-  pg_create_physical_replication_slot ('slot_name', isDummyStandby)

   Description: Creates a physical replication slot.

   Parameter:

   -  slot_name

      Indicates the name of the streaming replication slot.

      Value range: a string, supporting only letters, digits, and the following special characters: \_?.-

   -  isDummyStandby

      Indicates whether the replication slot is the secondary one.

      Value range: a boolean value, **true** or **false**

   Return type: name, text

   Note: The first return value is the slot name, and the second is the start LSN position for decoding in the physical replication slot.

-  pg_get_replication_slots()

   Description: Displays information about all replication slots on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 1** pg_get_replication_slots() fields

      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+
      | Field                 | Type                  | Description                                                                           |
      +=======================+=======================+=======================================================================================+
      | slot_name             | text                  | Replication slot name                                                                 |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+
      | plugin                | name                  | Name of the output plug-in of the logical replication slot                            |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+
      | slot_type             | text                  | Replication slot type                                                                 |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+
      | datoid                | oid                   | Replication slot's database OID                                                       |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+
      | active                | boolean               | Whether the replication slot is active                                                |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+
      | xmin                  | xid                   | Transaction ID of the replication slot                                                |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+
      | catalog_xmin          | text                  | ID of the earliest-decoded transaction corresponding to the logical replication slot. |
      |                       |                       |                                                                                       |
      | restart_lsn           | text                  | Xlog file information on the replication slot.                                        |
      |                       |                       |                                                                                       |
      | dummy_standby         | boolean               | Indicates whether the replication slot is the secondary one.                          |
      +-----------------------+-----------------------+---------------------------------------------------------------------------------------+

-  pg_drop_replication_slot('slot_name')

   Description: Deletes a streaming replication slot.

   Parameter:

   -  slot_name

      Indicates the name of the streaming replication slot.

      Value range: a string, supporting only letters, digits, and the following special characters: \_?-.

   Return type: void

-  .. _en-us_topic_0000001099150748__li11712645125:

   pg_logical_slot_peek_changes('slot_name', 'LSN', upto_nchanges, 'options_name', 'options_value')

   Description: Performs decoding but does not go to the next streaming replication slot. (The decoding result will be returned again on future calls.)

   Parameter:

   -  slot_name

      Indicates the name of the streaming replication slot.

      Value range: a string, supporting only letters, digits, and the following special characters: \_?-.

   -  LSN

      Indicates a target LSN. Decoding is performed only when an LSN is less than or equal to this value.

      Value range: a string, in the format of xlogid/xrecoff, for example, '1/2AAFC60' (If this parameter is set to **NULL**, the target LSN indicating the end position of decoding is not specified.)

   -  upto_nchanges

      Indicates the number of decoded records (including the **begin** and **commit** timestamps). Assume that there are three transactions, which involve 3, 5, and 7 records, respectively. If **upto_nchanges** is **4**, 8 records of the first two transactions will be decoded. Specifically, decoding is stopped when the number of decoded records exceeds 4 after decoding in the first two transactions is finished.

      Value range: a non-negative integer

      .. note::

         If any of the **LSN** and **upto_nchanges** values are reached, decoding ends.

   -  options (optional)

      -  include-xids

         Indicates whether the decoded **data** column contains XID information.

         Valid value: **0** and **1**. The default value is **1**.

         -  **0**: The decoded **data** column does not contain XID information.
         -  **1**: The decoded **data** column contains XID information.

      -  skip-empty-xacts

         Indicates whether to ignore empty transaction information during decoding.

         Valid value: **0** and **1**. The default value is **0**.

         -  **0**: The empty transaction information is not ignored during decoding.
         -  **1**: The empty transaction information is ignored during decoding.

      -  include-timestamp

         Indicates whether decoding information contains the **commit** timestamp.

         Valid value: **0** and **1**. The default value is **0**.

         -  **0**: The decoding information does not contain the **commit** timestamp.
         -  **1**: The decoding information contains the **commit** timestamp.

   Return type: text, uint, text

   Note: The function returns the decoding result. Each decoding result contains three columns, corresponding to the above return types and indicating the LSN position, XID, and decoded content, respectively.

-  pg_logical_slot_get_changes('slot_name', 'LSN', upto_nchanges, 'options_name', 'options_value')

   Description: Performs decoding and goes to the next streaming replication slot.

   Parameter: This function has the same parameters as **pg_logical_slot_peek_changes**. For details, see :ref:`pg_logical_slot_peek_ch... <en-us_topic_0000001099150748__li11712645125>`.

-  pg_replication_slot_advance ('slot_name', 'LSN')

   Description: Directly goes to the streaming replication slot for a specified LSN, without outputting any decoding result.

   Parameter:

   -  slot_name

      Indicates the name of the streaming replication slot.

      Value range: a string, supporting only letters, digits, and the following special characters: \_?-.

   -  LSN

      Indicates a target LSN. Next decoding will be performed only in transactions whose commission position is greater than this value. If an input LSN is smaller than the position recorded in the current streaming replication slot, the function directly returns. If the input LSN is greater than the LSN of the current physical log, the latter LSN will be directly used for decoding.

      Value range: a string, in the format of xlogid/xrecoff

   Return type: name, text

   Note: A return result contains the slot name and LSN that is actually used for decoding.

-  pg_stat_get_data_senders()

   Description: Displays statistics about replication sending threads on all data page on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 2** pg_stat_get_data_senders() fields

      +------------------------+--------------------------+------------------------------------------------------------+
      | Field                  | Type                     | Description                                                |
      +========================+==========================+============================================================+
      | pid                    | bigint                   | Thread PID                                                 |
      +------------------------+--------------------------+------------------------------------------------------------+
      | sender_pid             | integer                  | Current sender PID                                         |
      +------------------------+--------------------------+------------------------------------------------------------+
      | local_role             | text                     | Local role                                                 |
      +------------------------+--------------------------+------------------------------------------------------------+
      | peer_role              | text                     | Peer role                                                  |
      +------------------------+--------------------------+------------------------------------------------------------+
      | state                  | text                     | Current sender's replication status                        |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_start          | timestamp with time zone | Startup time of a catchup task                             |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_end            | timestamp with time zone | End time of a catchup task                                 |
      +------------------------+--------------------------+------------------------------------------------------------+
      | queue_size             | text                     | Data queue size                                            |
      +------------------------+--------------------------+------------------------------------------------------------+
      | queue_lower_tail       | text                     | Position of data queue tail 1                              |
      +------------------------+--------------------------+------------------------------------------------------------+
      | queue_header           | text                     | Position of data queue header                              |
      +------------------------+--------------------------+------------------------------------------------------------+
      | queue_upper_tail       | text                     | Position of data queue tail 2                              |
      +------------------------+--------------------------+------------------------------------------------------------+
      | send_position          | text                     | Sending position of the sender                             |
      +------------------------+--------------------------+------------------------------------------------------------+
      | receive_position       | text                     | Receiving position of the receiver                         |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_type           | text                     | Catchup task type, full or incremental                     |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_bcm_filename   | text                     | BCM file executed by the current catchup task              |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_bcm_finished   | integer                  | Number of BCM files completed by a catchup task            |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_bcm_total      | integer                  | Total number of BCM files to be operated by a catchup task |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_percent        | text                     | Completion percentage of a catchup task                    |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_remaining_time | text                     | Estimated remaining time of a catchup task                 |
      +------------------------+--------------------------+------------------------------------------------------------+

-  pg_stat_get_wal_senders()

   Description: Displays statistics about replication sending threads on all WALs on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 3** pg_stat_get_wal_senders() fields

      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | Field                      | Type                     | Description                                                                                             |
      +============================+==========================+=========================================================================================================+
      | pid                        | bigint                   | Thread PID                                                                                              |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sender_pid                 | integer                  | Current sender PID                                                                                      |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | local_role                 | text                     | Local role                                                                                              |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | peer_role                  | text                     | Peer role                                                                                               |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | peer_state                 | text                     | Peer status                                                                                             |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | state                      | text                     | Current sender's replication status                                                                     |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | catchup_start              | timestamp with time zone | Startup time of a catchup task                                                                          |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | catchup_end                | timestamp with time zone | End time of a catchup task                                                                              |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sender_sent_location       | text                     | Location where the sender sends LSNs                                                                    |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sender_write_location      | text                     | Location where the sender writes LSNs                                                                   |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sender_flush_location      | text                     | Location where the sender flushes LSNs                                                                  |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sender_replay_location     | text                     | Location where the sender replays LSNs                                                                  |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | receiver_received_location | text                     | Location where the receiver receives LSNs                                                               |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | receiver_write_location    | text                     | Location where the receiver writes LSNs                                                                 |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | receiver_flush_location    | text                     | Location where the receiver flushes LSNs                                                                |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | receiver_replay_location   | text                     | Location where the receiver replays LSNs                                                                |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sync_percent               | text                     | Specifies the synchronization percentage.                                                               |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sync_state                 | text                     | Synchronization state (asynchronous duplication, synchronous duplication, or potential synchronization) |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sync_priority              | integer                  | Priority of synchronous duplication (**0** indicates asynchronization)                                  |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | sync_most_available        | text                     | Whether to block the active node when the synchronization on the standby node fails                     |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
      | channel                    | text                     | WALSender channel information                                                                           |
      +----------------------------+--------------------------+---------------------------------------------------------------------------------------------------------+

-  pg_stat_get_wal_receiver()

   Description: Displays statistics about replication receiving threads on all WALs on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 4** pg_stat_get_wal_receiver()

      +----------------------------+---------+-------------------------------------------+
      | Field                      | Type    | Description                               |
      +============================+=========+===========================================+
      | receiver_pid               | integer | Current receiver PID                      |
      +----------------------------+---------+-------------------------------------------+
      | local_role                 | text    | Local role                                |
      +----------------------------+---------+-------------------------------------------+
      | peer_role                  | text    | Peer role                                 |
      +----------------------------+---------+-------------------------------------------+
      | peer_state                 | text    | Peer status                               |
      +----------------------------+---------+-------------------------------------------+
      | state                      | text    | Current receiver's replication status     |
      +----------------------------+---------+-------------------------------------------+
      | sender_sent_location       | text    | Location where the sender sends LSNs      |
      +----------------------------+---------+-------------------------------------------+
      | sender_write_location      | text    | Location where the sender writes LSNs     |
      +----------------------------+---------+-------------------------------------------+
      | sender_flush_location      | text    | Location where the sender flushes LSNs    |
      +----------------------------+---------+-------------------------------------------+
      | sender_replay_location     | text    | Location where the sender replays LSNs    |
      +----------------------------+---------+-------------------------------------------+
      | receiver_received_location | text    | Location where the receiver receives LSNs |
      +----------------------------+---------+-------------------------------------------+
      | receiver_write_location    | text    | Location where the receiver writes LSNs   |
      +----------------------------+---------+-------------------------------------------+
      | receiver_flush_location    | text    | Location where the receiver flushes LSNs  |
      +----------------------------+---------+-------------------------------------------+
      | receiver_replay_location   | text    | Location where the receiver replays LSNs  |
      +----------------------------+---------+-------------------------------------------+
      | sync_percent               | text    | Specifies the synchronization percentage. |
      +----------------------------+---------+-------------------------------------------+
      | channel                    | text    | WALReceiver channel information           |
      +----------------------------+---------+-------------------------------------------+

-  pg_stat_get_stream_replications()

   Description: Displays information about all replication statistics on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 5** pg_stat_get_stream_replications()

      ================== ======= =====================
      Field              Type    Description
      ================== ======= =====================
      local_role         text    Local role
      static_connections integer Connection statistics
      db_state           text    Database status
      detail_information text    Detail information
      ================== ======= =====================

-  pg_stat_xlog_space()

   Description: Displays the Xlog space usage on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 6** pg_stat_xlog_space()

      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | Column     | Type   | Description                                                                                                                                |
      +============+========+============================================================================================================================================+
      | xlog_files | bigint | Number of all identified xlog files in the **pg_xlog** directory, excluding the **backup** and **archive_status** subdirectories.          |
      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | xlog_size  | bigint | Total size (MB) of all identified xlog files in the **pg_xlog** directory, excluding the **backup** and **archive_status** subdirectories. |
      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | other_size | bigint | Total size (MB) of files in the **backup** and **archive_status** subdirectories of the **pg_xlog** directory.                             |
      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+

-  pgxc_stat_xlog_space()

   Description: Displays the Xlog space usage on all active DNs.

   Return type: record

   The following information is returned:

   .. table:: **Table 7** pgxc_stat_xlog_space()

      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | Column     | Type   | Description                                                                                                                                |
      +============+========+============================================================================================================================================+
      | node_name  | name   | Node name                                                                                                                                  |
      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | xlog_files | bigint | Number of all identified xlog files in the **pg_xlog** directory, excluding the **backup** and **archive_status** subdirectories.          |
      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | xlog_size  | bigint | Total size (MB) of all identified xlog files in the **pg_xlog** directory, excluding the **backup** and **archive_status** subdirectories. |
      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | other_size | bigint | Total size (MB) of files in the **backup** and **archive_status** subdirectories of the **pg_xlog** directory.                             |
      +------------+--------+--------------------------------------------------------------------------------------------------------------------------------------------+
