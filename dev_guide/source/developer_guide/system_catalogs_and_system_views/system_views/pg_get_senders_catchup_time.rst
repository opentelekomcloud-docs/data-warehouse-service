:original_name: dws_04_0730.html

.. _dws_04_0730:

PG_GET_SENDERS_CATCHUP_TIME
===========================

**PG_GET_SENDERS_CATCHUP_TIME** displays the catchup information of the currently active primary/standby instance sending thread on a single DN.

.. table:: **Table 1** PG_GET_SENDERS_CATCHUP_TIME columns

   +------------------------+--------------------------+------------------------------------------------------------+
   | Name                   | Type                     | Description                                                |
   +========================+==========================+============================================================+
   | pid                    | bigint                   | Current sender thread ID                                   |
   +------------------------+--------------------------+------------------------------------------------------------+
   | lwpid                  | integer                  | Current sender lwpid                                       |
   +------------------------+--------------------------+------------------------------------------------------------+
   | local_role             | text                     | Local role                                                 |
   +------------------------+--------------------------+------------------------------------------------------------+
   | peer_role              | text                     | Peer role                                                  |
   +------------------------+--------------------------+------------------------------------------------------------+
   | state                  | text                     | Current sender's replication status                        |
   +------------------------+--------------------------+------------------------------------------------------------+
   | type                   | text                     | Current sender type                                        |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_start          | timestamp with time zone | Startup time of a catchup task                             |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_end            | timestamp with time zone | End time of a catchup task                                 |
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
