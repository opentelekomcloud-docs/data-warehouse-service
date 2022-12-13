:original_name: dws_01_10134.html

.. _dws_01_10134:

Deleting an Automated Snapshot
==============================

Only GaussDB(DWS) can delete automated snapshots. You cannot delete them manually.

GaussDB(DWS) deletes an automated snapshot if:

-  The retention period of the snapshot ends.
-  The automated snapshot function is disabled for a cluster. For details, see :ref:`Configuring an Automated Snapshot Policy <dws_01_0089>`.
-  The cluster is deleted.

.. caution::

   If you disable automated snapshot, GaussDB(DWS) will stop taking snapshots and delete the existing automated snapshots of the corresponding cluster. Exercise caution when performing this operation.
