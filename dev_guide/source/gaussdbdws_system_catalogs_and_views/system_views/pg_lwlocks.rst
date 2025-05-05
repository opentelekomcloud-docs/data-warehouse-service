:original_name: dws_04_1411.html

.. _dws_04_1411:

PG_LWLOCKS
==========

**PG_LWLOCKS** provides information on lightweight locks currently held or being waited for by the current instance. This view is supported only by 9.1.0.200 and later cluster versions.

.. table:: **Table 1** PG_LWLOCKS columns

   +--------------+---------+---------------------------------------------------------------------------+
   | Column       | Type    | Description                                                               |
   +==============+=========+===========================================================================+
   | pid          | Bigint  | ID of the backend thread.                                                 |
   +--------------+---------+---------------------------------------------------------------------------+
   | query_id     | Bigint  | ID of a query.                                                            |
   +--------------+---------+---------------------------------------------------------------------------+
   | lwtid        | Integer | Lightweight thread ID of the backend thread.                              |
   +--------------+---------+---------------------------------------------------------------------------+
   | reqlockid    | Integer | ID of the lightweight lock that is being requested by the current thread. |
   +--------------+---------+---------------------------------------------------------------------------+
   | reqlock      | Text    | Name of the lightweight lock corresponding to **reqlockid**.              |
   +--------------+---------+---------------------------------------------------------------------------+
   | heldlocknums | Integer | Number of lightweight locks obtained by the current thread.               |
   +--------------+---------+---------------------------------------------------------------------------+
   | heldlockid   | Integer | Lightweight lock ID obtained by the current thread.                       |
   +--------------+---------+---------------------------------------------------------------------------+
   | heldlock     | Text    | Name of the lightweight lock corresponding to **heldlockid**.             |
   +--------------+---------+---------------------------------------------------------------------------+
   | heldlockmode | Text    | Lightweight lock mode corresponding to **heldlockid**.                    |
   +--------------+---------+---------------------------------------------------------------------------+

Example
-------

Use the **PG_LWLOCKS** view to query information about lightweight locks that are being held or waiting for the current instance.

::

   SELECT * FROM pg_lwlocks;
          pid       |     query_id      | lwtid | reqlockid | reqlock | heldlocknums | heldlockid |      heldlock      | heldlockmode
   -----------------+-------------------+-------+-----------+---------+--------------+------------+--------------------+--------------
    139810224192480 |                 0 | 54842 |           |         |            1 |          7 | WALWriteLock       | Exclusive
    139810224199520 | 78250043526306022 | 54963 |           |         |            1 |     193860 | BUFFER_POOL_LWLOCK | Exclusive
   (2 rows)
