:original_name: dws_01_0118.html

.. _dws_01_0118:

Viewing Operation Logs on the GaussDB(DWS) Console
==================================================

Enabling CTS
------------

A tracker will be automatically created after CTS is enabled. All traces recorded by CTS are associated with a tracker. Currently, only one tracker can be created for each account.

#. Log in to the management console, choose **Service List** > **Management & Governance** > **Cloud Trace Service**. The CTS management console is displayed.

#. In the navigation tree on the left, choose **Cloud Trace Service > Tracker**.

#. Enable CTS.

   If you are a first-time CTS user and do not have any trackers in the tracker list, enable CTS first. For details, see "Getting Started > Enabling CTS" in the *Cloud Trace Service User Guide*.

   If you have enabled CTS, the system has automatically created a management tracker. Only one management tracker can be created and it cannot be deleted. You can also manually create a data tracker. For details, see "Tracker Management" > "Creating a Tracker" in the *Cloud Trace Service User Guide*.

Disabling the Audit Log Function
--------------------------------

If you want to disable the audit log function, disable the tracker in CTS.

#. Log in to the management console, choose **Service List** > **Management & Governance** > **Cloud Trace Service**. The CTS management console is displayed.

#. Disable the audit log function by disabling the tracker. To enable the audit log function again, you only need to enable the tracker.

   For details about how to enable or disable a tracker, see "Tracker Management > Disabling or Enabling a Tracker" in the *Cloud Trace Service User Guide*.

Key Operations
--------------

With CTS, you can record operations associated with GaussDB(DWS) for future query, audit, and backtracking.

.. note::

   -  The creation and deletion of automated snapshots are not performed by users, therefore not recorded in audit logs.
   -  There are many GaussDB(DWS) cluster operation events, but the table below only includes some frequently audited operations.

.. table:: **Table 1** GaussDB(DWS) operations that can be recorded by CTS

   +-------------------------------------------+------------------+----------------------------------+
   | Operation                                 | Resource         | Event Name                       |
   +===========================================+==================+==================================+
   | Creating a cluster                        | cluster          | createCluster                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting a cluster                        | cluster          | deleteCluster                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Performing a cluster inspection           | cluster          | createInspection                 |
   +-------------------------------------------+------------------+----------------------------------+
   | Stopping an inspection                    | cluster          | AbortInspection                  |
   +-------------------------------------------+------------------+----------------------------------+
   | Scaling out a cluster                     | cluster          | growCluster                      |
   +-------------------------------------------+------------------+----------------------------------+
   | Increasing the capacity of idle nodes     | cluster          | resizeWithFreeNodes              |
   +-------------------------------------------+------------------+----------------------------------+
   | Performing cluster redistribution         | cluster          | redistributeCluster              |
   +-------------------------------------------+------------------+----------------------------------+
   | Querying redistribution details           | cluster          | queryRedisInfo                   |
   +-------------------------------------------+------------------+----------------------------------+
   | Adding disk capacity                      | cluster          | executeDiskExpand                |
   +-------------------------------------------+------------------+----------------------------------+
   | Changing cluster flavors                  | cluster          | flavorResize                     |
   +-------------------------------------------+------------------+----------------------------------+
   | Restarting a cluster                      | cluster          | rebootCluster                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Performing a cluster switchover           | cluster          | activeStandySwitchover           |
   +-------------------------------------------+------------------+----------------------------------+
   | Resetting passwords                       | cluster          | resetPassword                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Restoring a cluster                       | cluster          | repairCluster                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating a cluster connection             | cluster          | createClusterConnection          |
   +-------------------------------------------+------------------+----------------------------------+
   | Modifying a cluster connection            | cluster          | modifyClusterConnection          |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting a cluster connection             | cluster          | deleteClusterConnection          |
   +-------------------------------------------+------------------+----------------------------------+
   | Resizing a cluster                        | cluster          | resizeCluster                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Binding or unbinding an EIP               | cluster          | bindOrUnbindEIP                  |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating or binding an ELB                | cluster          | createOrBindElb                  |
   +-------------------------------------------+------------------+----------------------------------+
   | Unbinding an ELB                          | cluster          | unbindElb                        |
   +-------------------------------------------+------------------+----------------------------------+
   | Adding a CN                               | cluster          | addCN                            |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting a CN                             | cluster          | deleteCN                         |
   +-------------------------------------------+------------------+----------------------------------+
   | Upgrading a cluster                       | cluster          | clusterUpdateMgr                 |
   +-------------------------------------------+------------------+----------------------------------+
   | Scaling in a cluster                      | cluster          | shrinkCluster                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating a resource management plan       | cluster          | addWorkloadPlan                  |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting a Resource Pool                  | cluster          | deleteWorkloadQueueInfo          |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating a resource pool                  | cluster          | addWorkloadQueueInfo             |
   +-------------------------------------------+------------------+----------------------------------+
   | Modifying cluster GUC parameters          | cluster          | updateClusterConfigurations      |
   +-------------------------------------------+------------------+----------------------------------+
   | Removing the read-only status             | cluster          | cancelReadonly                   |
   +-------------------------------------------+------------------+----------------------------------+
   | Modifying a maintenance window            | cluster          | modifyMaintenanceWindow          |
   +-------------------------------------------+------------------+----------------------------------+
   | Adding CN nodes in batches                | cluster          | batchCreateCn                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting CN nodes in batches              | cluster          | batchDeleteCn                    |
   +-------------------------------------------+------------------+----------------------------------+
   | Adding tags in batches                    | cluster          | batchCreateResourceTag           |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting tags in batches                  | cluster          | batchDeleteResourceTag           |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating a logical cluster                | cluster          | createLogicalCluster             |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting logical clusters                 | cluster          | deleteLogicalCluster             |
   +-------------------------------------------+------------------+----------------------------------+
   | Editing a logical cluster                 | cluster          | editLogicalCluster               |
   +-------------------------------------------+------------------+----------------------------------+
   | Restarting logical clusters               | cluster          | restartLogicalCluster            |
   +-------------------------------------------+------------------+----------------------------------+
   | Converting to a logical cluster           | cluster          | switchLogicalCluster             |
   +-------------------------------------------+------------------+----------------------------------+
   | Starting a cluster                        | cluster          | startCluster                     |
   +-------------------------------------------+------------------+----------------------------------+
   | Stopping a cluster                        | cluster          | stopCluster                      |
   +-------------------------------------------+------------------+----------------------------------+
   | Modifying the security group of a cluster | cluster          | changeSecurityGroup              |
   +-------------------------------------------+------------------+----------------------------------+
   | Changing the cluster time zone            | cluster          | modifyClusterTimezone            |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating a snapshot                       | backup           | createBackup                     |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting a snapshot                       | backup           | deleteBackup                     |
   +-------------------------------------------+------------------+----------------------------------+
   | Restoring a cluster                       | backup           | restoreCluster                   |
   +-------------------------------------------+------------------+----------------------------------+
   | Copying snapshots                         | backup           | copySnapshot                     |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting a snapshot policy                | backup           | deleteBackupPolicy               |
   +-------------------------------------------+------------------+----------------------------------+
   | Updating a snapshot policy                | backup           | updateClustersBackupPolicy       |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating a DR task                        | disasterRecovery | createDisasterRecovery           |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting a DR task                        | disasterRecovery | deleteDisasterRecovery           |
   +-------------------------------------------+------------------+----------------------------------+
   | Starting a DR task                        | disasterRecovery | startDisasterRecoveryAction      |
   +-------------------------------------------+------------------+----------------------------------+
   | Stopping a DR task                        | disasterRecovery | stopDisasterRecoveryAction       |
   +-------------------------------------------+------------------+----------------------------------+
   | Switching to the DR cluster               | disasterRecovery | switchoverDisasterRecoveryAction |
   +-------------------------------------------+------------------+----------------------------------+
   | Performing an exception switchover        | disasterRecovery | failoverDisasterRecoveryAction   |
   +-------------------------------------------+------------------+----------------------------------+
   | Performing DR                             | disasterRecovery | recoveryDisaster                 |
   +-------------------------------------------+------------------+----------------------------------+
   | Updating DR configurations                | disasterRecovery | updateRecoveryDisaster           |
   +-------------------------------------------+------------------+----------------------------------+
   | Querying DR details                       | disasterRecovery | disasterRecoveryOperate          |
   +-------------------------------------------+------------------+----------------------------------+
   | Setting security parameters               | configurations   | updateConfigurations             |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating an extended data source          | dataSource       | createExtDataSource              |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting an extended data source          | dataSource       | deleteExtDataSource              |
   +-------------------------------------------+------------------+----------------------------------+
   | Updating an extended data source          | dataSource       | updateExtDataSource              |
   +-------------------------------------------+------------------+----------------------------------+
   | Creating an MRS data source               | dataSource       | createExtDataSource              |
   +-------------------------------------------+------------------+----------------------------------+
   | Deleting an MRS data source               | dataSource       | deleteExtDataSource              |
   +-------------------------------------------+------------------+----------------------------------+
   | Updating an MRS data source               | dataSource       | updateExtDataSource              |
   +-------------------------------------------+------------------+----------------------------------+

Viewing Traces
--------------

#. Log in to the management console, choose **Service List** > **Management & Governance** > **Cloud Trace Service**. The CTS management console is displayed.

#. In the navigation pane on the left, choose **Trace List**.

#. Click the search box above the trace list and set the search criteria.

   The following filters are available:

   -  **Trace Name**: If you select this option, you also need to select a specific trace name.
   -  **Cloud Service**: Select **GaussDB(DWS)**.
   -  **Resource Type**: Select **All resource types** or specify a resource type.
   -  **Resource Name**: If you select this option, you also need to select or enter a specific resource name.
   -  **Resource ID**: If you select this option, you also need to select or enter a specific resource ID.
   -  **Operator**: Select a specific operator (at user level rather than tenant level).
   -  **Event ID**: If you select this option, you also need to select or enter an event ID.
   -  **Trace Status**: Available options include **All trace statuses**, **normal**, **warning**, and **incident**. You can only select one of them.
   -  **Enterprise Project ID**: If you select this option, you also need to select or enter a specific enterprise project ID.
   -  **Access Key ID**: If you select this option, you also need to select or enter a specific access key ID.

#. Click **Query**.

#. Click the name of the trace to be viewed. A window is displayed, showing the trace details.

   For details about key fields of a CTS trace, see "Trace References" > "Trace Structure" and "Trace References" > "Example Traces" in *Cloud Trace Service User Guide*.
