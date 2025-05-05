:original_name: dws_04_1412.html

.. _dws_04_1412:

PGXC_LWLOCKS
============

**PGXC_LWLOCK** offers details on lightweight locks that are currently held or being waited for by all instances in the cluster. This view is supported only by 9.1.0.200 and later cluster versions.

.. table:: **Table 1** PGXC_LWLOCKS columns

   +--------------+---------+--------------------------------------------------------------------------+
   | Column       | Type    | Description                                                              |
   +==============+=========+==========================================================================+
   | nodename     | Name    | Name of the node where the locked object resides                         |
   +--------------+---------+--------------------------------------------------------------------------+
   | pid          | Bigint  | ID of the backend thread                                                 |
   +--------------+---------+--------------------------------------------------------------------------+
   | query_id     | Bigint  | ID of a query                                                            |
   +--------------+---------+--------------------------------------------------------------------------+
   | lwtid        | Integer | Lightweight thread ID of the backend thread                              |
   +--------------+---------+--------------------------------------------------------------------------+
   | reqlockid    | Integer | ID of the lightweight lock that is being requested by the current thread |
   +--------------+---------+--------------------------------------------------------------------------+
   | reqlock      | Text    | Name of the lightweight lock corresponding to **reqlockid**              |
   +--------------+---------+--------------------------------------------------------------------------+
   | heldlocknums | Integer | Number of lightweight locks obtained by the current thread               |
   +--------------+---------+--------------------------------------------------------------------------+
   | heldlockid   | Integer | Lightweight lock ID obtained by the current thread                       |
   +--------------+---------+--------------------------------------------------------------------------+
   | heldlock     | Text    | Name of the lightweight lock corresponding to **heldlockid**             |
   +--------------+---------+--------------------------------------------------------------------------+
   | heldlockmode | Text    | Lightweight lock mode corresponding to **heldlockid**                    |
   +--------------+---------+--------------------------------------------------------------------------+

Example
-------

Use the **PGXC_LWLOCKS** view to get details on lightweight locks that are currently held or being waited for by all instances in the cluster.

::

   SELECT * FROM pgxc_lwlocks;
    nodename  |       pid       |     query_id      | lwtid | reqlockid | reqlock | heldlocknums | heldlockid |      heldlock      | heldlockmode
   -----------+-----------------+-------------------+-------+-----------+---------+--------------+------------+--------------------+--------------
    datanode1 | 139810224193360 | 78250043525924188 | 54844 |           |         |            1 |      76390 | BUFFER_POOL_LWLOCK | Shared
    datanode1 | 139810224198200 | 78250043525924886 | 54922 |           |         |            1 |     957438 | PGPROC_LWLOCK      | Exclusive
    datanode2 | 140262654050288 |                 0 | 54832 |           |         |            1 |          7 | WALWriteLock       | Exclusive
    datanode2 | 140262654052488 | 78250043525923195 | 54847 |           |         |            1 |      15862 | BUFFER_POOL_LWLOCK | Shared
   (4 rows)
