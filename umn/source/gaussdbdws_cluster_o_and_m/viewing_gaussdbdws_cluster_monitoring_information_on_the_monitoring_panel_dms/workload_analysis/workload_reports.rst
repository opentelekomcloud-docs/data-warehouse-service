:original_name: dws_01_00141.html

.. _dws_01_00141:

Workload Reports
================

You can create, download, and delete work diagnosis reports, and check historical workload diagnosis reports.

.. note::

   To create a workload report, obtain the required OBS bucket permissions first.

Checking Workload Reports
-------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster whose load needs to be analyzed.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**. Workload reports will be displayed.

Generating a Workload Report
----------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster whose load needs to be analyzed.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.
#. Click **Generate Report**. In the displayed dialog box, configure the following parameters and click **OK**:

   .. table:: **Table 1** Parameters for generating a report

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                             | Example Value         |
      +=======================+=========================================================================================================================================================================+=======================+
      | Report Name           | User-defined. Ensure that the name is unique and contains a maximum of 100 characters, including digits, letters, and underscores (_).                                  | test_show             |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Object Model          | The object types are as follows:                                                                                                                                        | node                  |
      |                       |                                                                                                                                                                         |                       |
      |                       | -  **node**: The performance data of a specified node will be provided.                                                                                                 |                       |
      |                       | -  **cluster**: The performance data of the entire cluster will be provided.                                                                                            |                       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Node Name             | User defined                                                                                                                                                            | dn_6005_l006          |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Content Type          | The options are as follows:                                                                                                                                             | all                   |
      |                       |                                                                                                                                                                         |                       |
      |                       | -  **summary**: A report contains only brief analysis and calculation results.                                                                                          |                       |
      |                       | -  **detail**: A report contains only detailed metric data.                                                                                                             |                       |
      |                       | -  **all**: A report contains content of both the summary and detail reports.                                                                                           |                       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Starting Snapshot     | User defined                                                                                                                                                            | ``-``                 |
      |                       |                                                                                                                                                                         |                       |
      |                       | .. note::                                                                                                                                                               |                       |
      |                       |                                                                                                                                                                         |                       |
      |                       |    The time of the starting snapshot start must be earlier than that of the ending snapshot.                                                                            |                       |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Ending Snapshot       | User defined                                                                                                                                                            | ``-``                 |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | OBS Bucket            | Bucket name, which is used to store reports.                                                                                                                            | test123               |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | OBS Path              | Storage directory, which can be customized. Multi-level directories can be separated by slashes (/) and cannot start with slashes (/). Up to 50 characters are allowed. | wdr                   |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

Downloading Workload Reports in Batches
---------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster whose load needs to be analyzed.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.
#. Select reports and click **Download**.

   .. note::

      Up to 10 report records can be downloaded at a time.

Deleting Workload Reports in Batches
------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster whose load needs to be analyzed.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.
#. Select reports and click **Delete**.

Deleting a Workload Diagnosis Report
------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster whose load needs to be analyzed.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.
#. Click **Delete** in the **Operation** column of a report to delete the report record and file.

Configuring Workload Report Parameters
--------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster whose load needs to be analyzed.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.
#. Click **Configure Report** in the upper right corner. In the displayed dialog box, set the report retention period and OBS parameters.
