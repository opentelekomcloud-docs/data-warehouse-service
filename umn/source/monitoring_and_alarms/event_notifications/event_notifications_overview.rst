:original_name: dws_01_0099.html

.. _dws_01_0099:

Event Notifications Overview
============================

Supported Event Types and Events
--------------------------------

Events are records of changes in the user's cluster status. Events can be triggered by user operations (such as audit events), or may be caused by cluster service status changes (for example, cluster repaired successfully or failed to repair the cluster). The following tables list the events and event types supported by GaussDB(DWS).

-  The following table lists the events whose **Event Source Category** is **Cluster**.

   .. table:: **Table 1** Events whose **Event Source Category** is **Cluster**

      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Event Type | Event Name                        | Event Severity | Event                                                              |
      +============+===================================+================+====================================================================+
      | Management | createClusterFail                 | Warning        | Failed to create the cluster.                                      |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | createClusterSuccess              | Normal         | Cluster created successfully.                                      |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | createCluster                     | Normal         | Cluster creation started.                                          |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | extendCluster                     | Normal         | Cluster scale-out started.                                         |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | extendClusterSuccess              | Normal         | Cluster scaled out successfully.                                   |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | extendClusterFail                 | Warning        | Failed to scale out the cluster.                                   |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | deleteClusterFail                 | Warning        | Failed to delete the cluster.                                      |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | deleteClusterSuccess              | Normal         | Cluster deleted successfully.                                      |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | deleteCluster                     | Normal         | Cluster deletion started.                                          |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | restoreClusterFail                | Warning        | Failed to restore the cluster.                                     |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | restoreClusterSuccess             | Normal         | Cluster restored successfully.                                     |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | restoreCluster                    | Normal         | Cluster restoration started.                                       |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | restartClusterFail                | Warning        | Failed to restart the cluster.                                     |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | restartClusterSuccess             | Normal         | Cluster restarted successfully.                                    |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | restartCluster                    | Normal         | Cluster restarted.                                                 |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | configureMRSExtDataSources        | Normal         | Configuration of MRS external data source for the cluster started. |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | configureMRSExtDataSourcesFail    | Warning        | Failed to configure the MRS external data source for the cluster.  |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | configureMRSExtDataSourcesSuccess | Normal         | MRS external data source configured successfully for the cluster.  |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | deleteMRSExtDataSources           | Normal         | Deletion of MRS external data source for the cluster started.      |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | deleteMRSExtDataSourcesFail       | Warning        | Failed to delete the MRS external data source for the cluster.     |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | deletedMRSExtDataSourcesSuccess   | Normal         | MRS external data source deleted successfully for the cluster.     |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | bindEipToCluster                  | Normal         | Bound an EIP to the cluster.                                       |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | bindEipToClusterFail              | Warning        | Failed to bind an EIP to the cluster.                              |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | unbindEipToCluster                | Normal         | Unbound an EIP from the cluster.                                   |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | unbindEipToClusterFail            | Warning        | Failed to unbind an EIP from the cluster.                          |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | refreshEipToCluster               | Normal         | Refreshed the cluster's EIP.                                       |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Management | refreshEipToClusterFail           | Warning        | Failed to refresh the cluster's EIP.                               |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Security   | resetPasswordFail                 | Warning        | Failed to reset the password.                                      |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Security   | resetPasswordSuccess              | Normal         | Password of the cluster reset successfully.                        |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Security   | updateConfiguration               | Normal         | Updating security parameters of the cluster started.               |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Security   | updateConfigurationFail           | Warning        | Failed to update security parameters of the cluster.               |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Security   | updateConfigurationSuccess        | Normal         | Security parameters of the cluster updated successfully.           |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Monitoring | repairCluster                     | Normal         | The node is faulty. Repairing the cluster starts.                  |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Monitoring | repairClusterFail                 | Warning        | Failed to repair the cluster.                                      |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+
      | Monitoring | repairClusterSuccess              | Normal         | Cluster repaired successfully.                                     |
      +------------+-----------------------------------+----------------+--------------------------------------------------------------------+

-  The following table lists the events whose **Event Source Category** is **Snapshot**.

   .. table:: **Table 2** Events whose **Event Source Category** is **Snapshot**

      +------------+---------------------+----------------+--------------------------------+
      | Event Type | Event Name          | Event Severity | Event                          |
      +============+=====================+================+================================+
      | Management | deleteBackup        | Normal         | Snapshot deleted successfully. |
      +------------+---------------------+----------------+--------------------------------+
      | Management | deleteBackupFail    | Warning        | Failed to delete the snapshot. |
      +------------+---------------------+----------------+--------------------------------+
      | Management | createBackup        | Normal         | Snapshot creation started.     |
      +------------+---------------------+----------------+--------------------------------+
      | Management | createBackupSuccess | Normal         | Snapshot created successfully. |
      +------------+---------------------+----------------+--------------------------------+
      | Management | createBackupFail    | Warning        | Failed to create the snapshot. |
      +------------+---------------------+----------------+--------------------------------+
