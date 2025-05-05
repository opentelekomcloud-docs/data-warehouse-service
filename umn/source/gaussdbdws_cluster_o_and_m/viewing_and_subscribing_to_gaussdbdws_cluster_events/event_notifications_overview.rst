:original_name: dws_01_0099.html

.. _dws_01_0099:

Event Notifications Overview
============================

Overview
--------

GaussDB(DWS) uses the Simple Message Notification (SMN) service to send notifications of GaussDB(DWS) events. The SMN function is only available by subscription. In a subscription, you need to specify one or more event filtering conditions. When an event that matches all filtering conditions occurs, GaussDB(DWS) sends a notification based on the subscription. The filter conditions include the **Event Type** (for example, **Management**, **Monitoring**, or **Security**), **Event Severity** (for example, **Normal** or **Warning**), and **Event Source Category** (for example, **Cluster** or **Snapshot**).

Supported Event Types and Events
--------------------------------

Events are records of changes in the user's cluster status. Events can be triggered by user operations (such as audit events), or may be caused by cluster service status changes (for example, cluster repaired successfully or failed to repair the cluster). The following tables list the events and event types supported by GaussDB(DWS).

-  The following table lists the events whose **Event Source Category** is **Cluster**.

   .. table:: **Table 1** Events whose **Event Source Category** is **Cluster**

      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Event Type | Event Name                        | Event Severity | Event                                                                |
      +============+===================================+================+======================================================================+
      | Management | createClusterFail                 | Warning        | Cluster creation failed.                                             |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | createClusterSuccess              | Normal         | The cluster is created.                                              |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | createCluster                     | Normal         | Cluster creation started.                                            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | extendCluster                     | Normal         | Cluster scale-out started.                                           |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | extendClusterSuccess              | Normal         | A cluster is scaled out.                                             |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | extendClusterFail                 | Warning        | Cluster scale-out failed.                                            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | deleteClusterFail                 | Warning        | Failed to delete the cluster.                                        |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | deleteClusterSuccess              | Normal         | The cluster is deleted.                                              |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | deleteCluster                     | Normal         | Cluster deletion started.                                            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | restoreClusterFail                | Warning        | Cluster restoration failed.                                          |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | restoreClusterSuccess             | Normal         | The cluster is restored.                                             |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | restoreCluster                    | Normal         | Cluster restoration started.                                         |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | restartClusterFail                | Warning        | Cluster restart failed.                                              |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | restartClusterSuccess             | Normal         | The cluster is restarted.                                            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | restartCluster                    | Normal         | Cluster restarted.                                                   |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | configureMRSExtDataSources        | Normal         | Configuration of MRS external data source for the cluster started.   |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | configureMRSExtDataSourcesFail    | Warning        | The cluster's MRS external data source configurations failed.        |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | configureMRSExtDataSourcesSuccess | Normal         | The MRS external data source of the cluster is configured.           |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | deleteMRSExtDataSources           | Normal         | Deletion of MRS external data source for the cluster started.        |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | deleteMRSExtDataSourcesFail       | Warning        | The deletion of the MRS external data source for the cluster failed. |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | deletedMRSExtDataSourcesSuccess   | Normal         | MRS external data source is deleted.                                 |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | bindEipToCluster                  | Normal         | An EIP is bound to the cluster.                                      |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | bindEipToClusterFail              | Warning        | Cluster EIP binding failed.                                          |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | unbindEipToCluster                | Normal         | The EIP is unbound from the cluster.                                 |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | unbindEipToClusterFail            | Warning        | Cluster EIP unbinding failed                                         |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | refreshEipToCluster               | Normal         | The cluster's EIP is refreshed.                                      |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Management | refreshEipToClusterFail           | Warning        | Cluster EIP refreshing failed.                                       |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Security   | resetPasswordFail                 | Warning        | The password reset attempt was unsuccessful.                         |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Security   | resetPasswordSuccess              | Normal         | The cluster password has been reset.                                 |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Security   | updateConfiguration               | Normal         | Start to update cluster security parameters.                         |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Security   | updateConfigurationFail           | Warning        | Cluster security parameter update failed.                            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Security   | updateConfigurationSuccess        | Normal         | Cluster security parameters were updated.                            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Monitoring | repairCluster                     | Normal         | The node is faulty and the cluster starts to be repaired.            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Monitoring | repairClusterFail                 | Warning        | Cluster repairing failed.                                            |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+
      | Monitoring | repairClusterSuccess              | Normal         | The cluster is repaired.                                             |
      +------------+-----------------------------------+----------------+----------------------------------------------------------------------+

-  The following table lists the events whose **Event Source Category** is **Snapshot**.

   .. table:: **Table 2** Events whose **Event Source Category** is **Snapshot**

      +------------+---------------------+----------------+--------------------------------+
      | Event Type | Event Name          | Event Severity | Event                          |
      +============+=====================+================+================================+
      | Management | deleteBackup        | Normal         | The snapshot is deleted.       |
      +------------+---------------------+----------------+--------------------------------+
      | Management | deleteBackupFail    | Warning        | Snapshot deletion failed.      |
      +------------+---------------------+----------------+--------------------------------+
      | Management | createBackup        | Normal         | The snapshot is being created. |
      +------------+---------------------+----------------+--------------------------------+
      | Management | createBackupSuccess | Normal         | The snapshot is created.       |
      +------------+---------------------+----------------+--------------------------------+
      | Management | createBackupFail    | Warning        | Snapshot creation failed.      |
      +------------+---------------------+----------------+--------------------------------+

-  The following table lists the events whose **Event Source Category** is **DR**.

   .. table:: **Table 3** Events whose **Event Source Category** is **DR**.

      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Event Type | Event Name                                   | Event Severity | Event                                                    |
      +============+==============================================+================+==========================================================+
      | Management | beginCreateDisasterRecovery                  | Normal         | The DR task is being created.                            |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | createDisasterRecoverySuccess                | Normal         | The DR task is created.                                  |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | createDisasterRecoveryFail                   | Warning        | DR task creation failed.                                 |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | beginStartDisasterRecovery                   | Normal         | The DR task starts.                                      |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | startDisasterRecoverySuccess                 | Normal         | The DR task started successfully.                        |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | startDisasterRecoveryFail                    | Warning        | The DR task was unable to start.                         |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | beginStopDisasterRecovery                    | Normal         | The DR task is being stopped.                            |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | stopDisasterRecoverySuccess                  | Normal         | DR stopped successfully.                                 |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | stopDisasterRecoveryFail                     | Warning        | The DR task could not be stopped.                        |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | beginSwitchoverDisasterRecovery              | Normal         | The DR switchover starts.                                |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | switchoverDisasterRecoverySuccess            | Normal         | The DR switchover is successful.                         |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | switchoverDisasterRecoveryFail               | Warning        | The DR switchover failed.                                |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | beginDeleteDisasterRecovery                  | Normal         | The DR task is being deleted.                            |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | deleteDisasterRecoverySuccess                | Normal         | The DR task is deleted.                                  |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | deleteDisasterRecoveryFail                   | Warning        | The DR task deletion failed.                             |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | disasterRecoveryAbnormal                     | Warning        | The DR task runs abnormally.                             |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | beginFailoverDisasterRecovery                | Normal         | The abnormal switchover starts.                          |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | failoverDisasterRecoverySuccess              | Normal         | The abnormal switchover is successful.                   |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | failoverDisasterRecoveryFail                 | Warning        | The abnormal switchover fails.                           |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | beginRecoveryDisaster                        | Normal         | The disaster recovery starts.                            |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | recoveryDisasterSuccess                      | Normal         | The disaster recovery is successful.                     |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | recoveryDisasterFail                         | Warning        | The disaster recovery fails.                             |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | emptyDisasterRecovery                        | Warning        | No DR table exists in the current DR object.             |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+
      | Management | switchoverContinueAsFailoverDisasterRecovery | Warning        | The DR switchover is degraded to an abnormal switchover. |
      +------------+----------------------------------------------+----------------+----------------------------------------------------------+

-  The following table lists the events whose event source type is data migration.

   .. table:: **Table 4** Events whose event source type is data migration

      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Event Type     | Event Name                                   | Event Severity | Event                                         |
      +================+==============================================+================+===============================================+
      | Data migration | dataMigrationApplicationDetectedAbnormal     | Warning        | The job task status is abnormal.              |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationApplicationReturnNormal         | Normal         | The job task is restored.                     |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationCreateApplication               | Normal         | Create a job task.                            |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationCreateCluster                   | Normal         | Start to create a data migration instance.    |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationCreateClusterFailed             | Warning        | Failed to create the data migration instance. |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationCreateClusterSuccess            | Normal         | The data migration instance is created.       |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationCreateConnection                | Normal         | Create a connection.                          |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationCreateMapping                   | Normal         | Create a table mapping configuration.         |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteApplication               | Normal         | Start to delete the job task.                 |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteApplicationFailed         | Warning        | Failed to delete the job.                     |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteApplicationSuccess        | Normal         | The job is deleted.                           |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteCluster                   | Normal         | Start to delete the data migration instance.  |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteClusterApplication        | Normal         | Start to delete the job task.                 |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteClusterApplicationFailed  | Warning        | Failed to delete the job.                     |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteClusterApplicationSuccess | Normal         | The job is deleted.                           |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteClusterFailed             | Warning        | Failed to delete the data migration instance. |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteClusterSuccess            | Normal         | The data migration instance is deleted.       |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteConnection                | Normal         | Delete the connection configuration.          |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDeleteMapping                   | Normal         | Delete the table mapping configuration.       |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationDialsConnection                 | Normal         | Test the connection configuration.            |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationModifyConnection                | Normal         | Modify the connection configuration.          |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationModifyMapping                   | Normal         | Modify the table mapping configuration.       |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationStartApplication                | Normal         | Start the job task.                           |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationStartApplicationFailed          | Warning        | Failed to start the job.                      |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationStartApplicationSuccess         | Normal         | The job task is started.                      |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationStopApplication                 | Normal         | Start to stop the job.                        |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationStopApplicationFailed           | Warning        | Failed to stop the job.                       |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
      | Data migration | dataMigrationStopApplicationSuccess          | Normal         | The job task is stopped.                      |
      +----------------+----------------------------------------------+----------------+-----------------------------------------------+
