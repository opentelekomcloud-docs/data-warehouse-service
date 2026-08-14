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

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002591529642.png
