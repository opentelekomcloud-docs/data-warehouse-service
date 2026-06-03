:original_name: dws_01_0099.html

.. _dws_01_0099:

Viewing DWS Cluster Events
==========================

Events are records of changes in the user's cluster status. Events can be triggered by user operations (such as audit events), or may be caused by cluster service status changes (for example, cluster repaired successfully or failed to repair the cluster). DWS supports the following event types: :ref:`Cluster Events <en-us_topic_0000002356728349__en-us_topic_0000001422799501_section29719263215>`, :ref:`Snapshot Events <en-us_topic_0000002356728349__en-us_topic_0000001422799501_section20739116335>`, :ref:`Disaster Recovery Events <en-us_topic_0000002356728349__en-us_topic_0000001422799501_section164090321335>`, and :ref:`Data Migration Events <en-us_topic_0000002356728349__en-us_topic_0000001422799501_section9501927153414>`. Generally, normal events do not require human intervention. For warning events, you need to check whether related operations are normal.

Viewing Events
--------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Events**.

   On the **Events** page, all the events that have occurred in all clusters or snapshots are displayed by default.

   You can sort the events in descending or ascending order by clicking |image1| next to **Time**.

   You can search for events by time, event, event level, event source, event source type, or event type using the search box at the top of the event list.

.. _en-us_topic_0000002356728349__en-us_topic_0000001422799501_section29719263215:

Cluster Events
--------------

.. table:: **Table 1** Cluster events

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
   | Management | restartCluster                    | Normal         | Cluster restart started.                                             |
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
   | Management | unbindEipToClusterFail            | Warning        | Cluster EIP unbinding failed.                                        |
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

.. _en-us_topic_0000002356728349__en-us_topic_0000001422799501_section20739116335:

Snapshot Events
---------------

.. table:: **Table 2** Snapshot events

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

.. _en-us_topic_0000002356728349__en-us_topic_0000001422799501_section164090321335:

Disaster Recovery Events
------------------------

.. table:: **Table 3** Disaster recovery events

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

.. _en-us_topic_0000002356728349__en-us_topic_0000001422799501_section9501927153414:

Data Migration Events
---------------------

.. table:: **Table 4** Data migration events

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

.. |image1| image:: /_static/images/en-us_image_0000002323336968.png
