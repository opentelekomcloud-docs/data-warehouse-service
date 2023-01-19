:original_name: dws_01_0035.html

.. _dws_01_0035:

Read-only Status
================

No database operation is allowed on a read-only cluster. Cancel the read-only status on the management console.

Impact on the System
--------------------

-  You can cancel the read-only status only when a cluster is read-only.
-  When a cluster is in read-only status, stop the write tasks to prevent data loss caused by used up disk space.
-  After the read-only status is canceled, clear the data as soon as possible to prevent the cluster from entering the read-only status again after a period of time.

Canceling Read-only Status
--------------------------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**.

   All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Cancel Readonly**.

#. In the dialog box that is displayed, click **OK** to confirm and cancel the read-only status for the cluster.
