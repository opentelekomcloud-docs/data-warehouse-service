:original_name: dws_04_0730.html

.. _dws_04_0730:

PG_GET_SENDERS_CATCHUP_TIME
===========================

**PG_GET_SENDERS_CATCHUP_TIME** displays the catchup information of the currently active primary/standby instance sending thread on a single DN.

.. table:: **Table 1** PG_GET_SENDERS_CATCHUP_TIME columns

   +------------------------+--------------------------+------------------------------------------------------------+
   | Column                 | Type                     | Description                                                |
   +========================+==========================+============================================================+
   | pid                    | Bigint                   | Current sender thread ID                                   |
   +------------------------+--------------------------+------------------------------------------------------------+
   | lwpid                  | Integer                  | Current sender lwpid                                       |
   +------------------------+--------------------------+------------------------------------------------------------+
   | local_role             | Text                     | Local role                                                 |
   +------------------------+--------------------------+------------------------------------------------------------+
   | peer_role              | Text                     | Peer role                                                  |
   +------------------------+--------------------------+------------------------------------------------------------+
   | state                  | Text                     | Current sender's replication status                        |
   +------------------------+--------------------------+------------------------------------------------------------+
   | type                   | Text                     | Current sender type                                        |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_start          | Timestamp with time zone | Startup time of a catchup task                             |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_end            | Timestamp with time zone | End time of a catchup task                                 |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_type           | Text                     | Catchup task type, full or incremental                     |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_bcm_filename   | Text                     | BCM file executed by the current catchup task              |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_bcm_finished   | Integer                  | Number of BCM files completed by a catchup task            |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_bcm_total      | Integer                  | Total number of BCM files to be operated by a catchup task |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_percent        | Text                     | Completion percentage of a catchup task                    |
   +------------------------+--------------------------+------------------------------------------------------------+
   | catchup_remaining_time | Text                     | Estimated remaining time of a catchup task                 |
   +------------------------+--------------------------+------------------------------------------------------------+
