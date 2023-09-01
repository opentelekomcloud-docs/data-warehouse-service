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

   If you have enabled CTS, the system has automatically created a management tracker. Only one management tracker can be created and it cannot be deleted. You can also manually create a data tracker. For details, see **Managing Trackers** > **Creating a Tracker** in the *Cloud Trace Service User Guide*.

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

#. In the upper right corner of the trace list, click **Filter** to set the search criteria.

   The following filters are available:

   -  **Trace Source**, **Resource Type**, and **Search By**

      -  **Trace Source**: Select **GaussDB(DWS)**.
      -  **Resource Type**: Select **All resource types** or specify a resource type.
      -  **Search By**: Select **All filters** or any of the following options:

         -  **Trace name**: If you select this option, you also need to select a specific trace name.
         -  **Resource ID**: If you select this option, you also need to select or enter a specific resource ID.
         -  **Resource name**: If you select this option, you also need to select or enter a specific resource name.

   -  **Operator**: Select a specific operator (at user level rather than tenant level).

   -  **Trace Status**: Available options include **All trace statuses**, **normal**, **warning**, and **incident**. You can only select one of them.

   -  **Start Date** and **End Date**: You can specify the time period to query traces.


      .. figure:: /_static/images/en-us_image_0000001517754605.png
         :alt: **Figure 1** Querying traces

         **Figure 1** Querying traces

#. Click **Query**.

#. Click |image1| on the left of the trace to be queried to extend its details.


   .. figure:: /_static/images/en-us_image_0000001518034073.png
      :alt: **Figure 2** Traces

      **Figure 2** Traces

#. Locate the row containing the target trace and click **View Trace** in the **Operation** column.


   .. figure:: /_static/images/en-us_image_0000001466754906.png
      :alt: **Figure 3** Viewing a trace

      **Figure 3** Viewing a trace

   For details about the key fields in the CTS trace structure, see "Trace References > Trace Structure" and "Trace References > Example Traces" in the *Cloud Trace Service User Guide*.

.. |image1| image:: /_static/images/en-us_image_0000001517754601.png
