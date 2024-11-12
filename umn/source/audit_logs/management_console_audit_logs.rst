:original_name: dws_01_0118.html

.. _dws_01_0118:

Management Console Audit Logs
=============================

Enabling CTS
------------

A tracker will be automatically created after CTS is enabled. All traces recorded by CTS are associated with a tracker. Currently, only one tracker can be created for each account.

#. Log in to the management console, choose **Service List** > **Management & Governance** > **Cloud Trace Service**. The CTS management console is displayed.

#. In the navigation tree on the left, choose **Cloud Trace Service > Tracker**.

#. Enable CTS.

   If you are a first-time CTS user and do not have any trackers in the tracker list, enable CTS first. For details, see "Getting Started > Enabling CTS" in the *Cloud Trace Service User Guide*.

   If you have enabled CTS, the system has automatically created a management tracker. Only one management tracker can be created and it cannot be deleted. You can also manually create a data tracker. For details, see "Tracker Management" > "Creating a Tracker" in *Cloud Trace Service User Guide*.

Disabling the Audit Log Function
--------------------------------

If you want to disable the audit log function, disable the tracker in CTS.

#. Log in to the management console, choose **Service List** > **Management & Governance** > **Cloud Trace Service**. The CTS management console is displayed.

#. Disable the audit log function by disabling the tracker. To enable the audit log function again, you only need to enable the tracker.

   For details about how to enable or disable a tracker, see "Tracker Management > Disabling or Enabling a Tracker" in the *Cloud Trace Service User Guide*.

Key Operations
--------------

With CTS, you can record operations associated with GaussDB(DWS) for later query, audit, and backtrack operations.

.. note::

   The creation and deletion of automatic snapshots are not performed by users, therefore not recorded in audit logs.

.. table:: **Table 1** GaussDB(DWS) operations that can be recorded by CTS

   ============================ ============== ====================
   Operation                    Resource       Event Name
   ============================ ============== ====================
   Creating/Restoring a cluster cluster        createCluster
   Deleting a cluster           cluster        deleteCluster
   Scaling out a cluster        cluster        resizeCluster
   Restarting a cluster         cluster        restartCluster
   Creating a snapshot          backup         createBackup
   Deleting a snapshot          backup         deleteBackup
   Setting security parameters  configurations updateConfigurations
   Creating an MRS data source  dataSource     createExtDataSource
   Deleting an MRS data source  dataSource     deleteExtDataSource
   Updating an MRS data source  dataSource     updateExtDataSource
   ============================ ============== ====================

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
