:original_name: dws_01_0035.html

.. _dws_01_0035:

Removing the Read-only Status
=============================

A cluster in read-only status does not allow write operations. You can remove this status on the management console. A cluster becomes read-only probably because of high disk usage. For how to solve this problem, see "High Disk Usage and Read Only Status" in *Data Warehouse Service (DWS) Troubleshooting Guide*.

.. note::

   -  The read-only status can be canceled for version 1.7.2 or later.
   -  In 8.2.0 and later versions, you can free up disk space by using **DROP/TRUNCATE TABLE** in a read-only cluster.

Impact on the System
--------------------

-  You can cancel the read-only status only when a cluster is read-only.
-  When a cluster is in read-only status, stop the write tasks to prevent data loss caused by used up disk space.
-  After the read-only status is canceled, clear the data as soon as possible to prevent the cluster from entering the read-only status again after a period of time.

Removing Read-only Status
-------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters** > **Dedicated Clusters**.

   All clusters are displayed by default.

#. In the row containing the cluster whose cluster status is **Read-only**, click **Cancel Read-only**.

#. In the dialog box that is displayed, click **OK** to confirm and remove the read-only status for the cluster.
