:original_name: dws_04_1037.html

.. _dws_04_1037:

PG_FINE_DR_INFO
===============

The **PG_FINE_DR_INFO** system catalog records the replay status of the fine-grained DR standby table. This system catalog is supported only by clusters of version 8.2.0.100 or later.

.. table:: **Table 1** PG_FINE_DR_INFO columns

   +---------------+--------------------------+----------------------------------------------------------------------------+
   | Name          | Type                     | Description                                                                |
   +===============+==========================+============================================================================+
   | oid           | oid                      | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +---------------+--------------------------+----------------------------------------------------------------------------+
   | relid         | oid                      | OID of the standby fine-grained DR table                                   |
   +---------------+--------------------------+----------------------------------------------------------------------------+
   | lastcsn       | xid                      | CSN of the last successful playback                                        |
   +---------------+--------------------------+----------------------------------------------------------------------------+
   | lastxmin      | xid                      | xmin of the last successful playback                                       |
   +---------------+--------------------------+----------------------------------------------------------------------------+
   | lastxmax      | xid                      | xmax of the last successful playback                                       |
   +---------------+--------------------------+----------------------------------------------------------------------------+
   | laststarttime | timestamp with time zone | Start time of the last successful playback                                 |
   +---------------+--------------------------+----------------------------------------------------------------------------+
   | lastendtime   | timestamp with time zone | End time of the last successful playback                                   |
   +---------------+--------------------------+----------------------------------------------------------------------------+

Examples
--------

Check the playback status of the standby table in the DR cluster.

::

   SELECT * FROM pg_fine_dr_info;
    relid | lastcsn | lastxmin | lastxmax |         laststarttime         |          lastendtime
   -------+---------+----------+----------+-------------------------------+-------------------------------
    21132 | 1251610 |  1251609 |  1251611 | 2023-01-04 20:51:58.375136+08 | 2023-01-04 20:51:58.393986+08
   (1 row)
