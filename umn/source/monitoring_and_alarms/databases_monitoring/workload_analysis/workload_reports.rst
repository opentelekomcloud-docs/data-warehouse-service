:original_name: dws_01_00141.html

.. _dws_01_00141:

Workload Reports
================

You can create, download, and delete work diagnosis reports, and check historical workload diagnosis reports.

.. note::

   To create a workload report, obtain the required OBS bucket permissions first.

Checking Workload Reports
-------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**. Workload reports will be displayed.

   |image1|

Generating a Workload Report
----------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.

#. Click **Generate Report**. In the displayed dialog box, configure the following parameters and click **OK**:

   -  **Report Name**: Enter a unique report name. The name can contain a maximum of 100 characters, including digits, letters, and underscores (_).)
   -  **Object Type**. Its value can be:

      -  **node**: The performance data of a specified node will be provided.
      -  **cluster**: The performance data of the entire cluster will be provided.

   -  **Node Name**: Select a node.
   -  **Content Type**. Its value can be:

      -  **summary**: A report contains only brief analysis and calculation results.
      -  **detail**: A report contains only detailed metric data.
      -  **all**: A report contains content of both the summary and detail reports.

   -  **Starting Snapshot**: Select a snapshot.
   -  **Ending Snapshot**: Select a snapshot.
   -  **OBS Bucket**: Select a bucket to store reports.
   -  **OBS Path**: A storage directory. Multiple levels of directories can be separated by slashes (/). The value cannot start with a slash (/). Up to 50 characters are allowed.

   |image2|

   |image3|

   |image4|

   .. note::

      The time of the starting snapshot start must be earlier than that of the ending snapshot.

Downloading Workload Reports in Batches
---------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.

#. Select reports and click **Download**.

   |image5|

   .. note::

      Up to 10 report records can be downloaded at a time.

Deleting Workload Reports in Batches
------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.

#. Select reports and click **Delete**.

   |image6|

Deleting a Workload Diagnosis Report
------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.

#. Click **Delete** in the **Operation** column of a report to delete the report record and file.

   |image7|

Configuring Workload Report Parameters
--------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane, choose **Workload Analysis** > **Workload Reports**.

#. Click **Configure Report** in the upper right corner. In the displayed dialog box, set the report retention period and OBS parameters.

   |image8|

.. |image1| image:: /_static/images/en-us_image_0000001467074166.png
.. |image2| image:: /_static/images/en-us_image_0000001466914298.png
.. |image3| image:: /_static/images/en-us_image_0000001466914306.png
.. |image4| image:: /_static/images/en-us_image_0000001518033841.png
.. |image5| image:: /_static/images/en-us_image_0000001517355345.png
.. |image6| image:: /_static/images/en-us_image_0000001517754377.png
.. |image7| image:: /_static/images/en-us_image_0000001517355349.png
.. |image8| image:: /_static/images/en-us_image_0000001517913953.png
