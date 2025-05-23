:original_name: dws_01_1019.html

.. _dws_01_1019:

Stopping Snapshot Creation
==========================

You can stop snapshot creation on the **Snapshots** page.

.. note::

   -  This feature is supported only in version 8.1.3.200 and later.
   -  If the snapshot is ready to complete, the command for stopping the snapshot will not take effect and the snapshot will end normally.

Precautions
-----------

Only the snapshots in the **Creating** state can be stopped. A snapshot creation task that just started or is about to complete cannot be stopped.

Procedure
---------

#. Log in to the GaussDB(DWS) console.

#. Choose **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page. Then, click **Snapshots**. All snapshots are displayed by default.

#. In the **Operation** column of a snapshot that is being created, and click **Cancel Creation**.

#. In the dialog box that is displayed, click **Yes** to stop the snapshot. The snapshot state will change to **Unavailable**.

   |image1|

.. |image1| image:: /_static/images/en-us_image_0000002167906444.png
