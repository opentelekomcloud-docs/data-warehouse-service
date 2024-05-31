:original_name: dws_01_10134.html

.. _dws_01_10134:

Deleting an Automated Snapshot
==============================

Only GaussDB(DWS) can delete automated snapshots; you cannot delete them manually.

GaussDB(DWS) deletes an automated snapshot if:

-  The retention period of the snapshot ends.
-  The cluster is deleted.

.. caution::

   To help users restore a cluster deleted by mistake, GaussDB(DWS) provides the following policies (supported only in 8.2.0 and later) for cluster snapshots:

   -  If the latest snapshot is an automated snapshot, it will be retained for one day.
   -  If the latest snapshot is a manual snapshot, the automated snapshot of the cluster will be deleted.
