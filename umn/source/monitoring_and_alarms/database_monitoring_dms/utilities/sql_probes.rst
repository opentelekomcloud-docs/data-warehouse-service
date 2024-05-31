:original_name: dws_01_01753.html

.. _dws_01_01753:

SQL Probes
==========

You can upload and verify SQL probes, execute probe tasks in one click, and periodically execute probe tasks. Alarms can be reported for timeout SQL probes. The following functions are supported:

-  :ref:`Adding a SQL Probe <en-us_topic_0000001707293797__en-us_topic_0000001372839446_section15317181252017>`
-  :ref:`Enabling or Disabling a SQL Probe <en-us_topic_0000001707293797__en-us_topic_0000001372839446_section151719344274>`
-  :ref:`Modifying an SQL Probe <en-us_topic_0000001707293797__en-us_topic_0000001372839446_section19477477271>`
-  :ref:`Deleting a SQL Probe <en-us_topic_0000001707293797__en-us_topic_0000001372839446_section16956310352>`
-  :ref:`Executing a SQL Probe in One Click <en-us_topic_0000001707293797__en-us_topic_0000001372839446_section13165101519425>`

.. note::

   -  The SQL probe is supported only in 8.1.1.300 and later versions. To use it in earlier versions, contact technical support.
   -  Only **SELECT** statements can be used as SQL probes.
   -  Up to 20 SQL probes can be configured.
   -  To create an SQL probe, you must have the GaussDB(DWS) FullAccess permission.
   -  To enable the SQL probe function, choose **Monitoring Settings** > **Monitoring Collection** and enable the **SQL Probe** metric. For details, see . The default collection frequency is 30s.

.. _en-us_topic_0000001707293797__en-us_topic_0000001372839446_section15317181252017:

Adding a SQL Probe
------------------

#. Log in to the GaussDB(DWS) console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane, choose **Utilities** > **SQL Probes**. Click **Add SQL Probe**.

   |image1|

#. Configure SQL probe parameters.

   -  **Probe Name**: Name of a probe.
   -  **Database**: Database where the probe SQL statement is to be executed.
   -  **SQL Statement**: Probe SQL statement to be executed. (Only **SELECT** statements are allowed).
   -  **Probe Threshold (ms)**: Timeout threshold of probe SQL execution.
   -  **Description**: Probe SQL statement description.

   |image2|

#. Confirm the SQL probe information and click **Confirm**.

.. _en-us_topic_0000001707293797__en-us_topic_0000001372839446_section151719344274:

Enabling or Disabling a SQL Probe
---------------------------------

#. Log in to the GaussDB(DWS) console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane on the left, choose **Utilities** > **SQL Probes**.

#. In the probe list, click **Enable** (or **Disable**) in the **Operation** column of a probe.

   |image3|

#. Confirm the information and click **OK**.

.. _en-us_topic_0000001707293797__en-us_topic_0000001372839446_section19477477271:

Modifying an SQL Probe
----------------------

#. Log in to the GaussDB(DWS) console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane on the left, choose **Utilities** > **SQL Probes**.

#. In the probe list, click **Modify** in the **Operation** column of a probe.

   |image4|

#. On the **Modify Probe** page, modify the SQL probe parameters as required and click **OK**.

   |image5|

.. _en-us_topic_0000001707293797__en-us_topic_0000001372839446_section16956310352:

Deleting a SQL Probe
--------------------

#. Log in to the GaussDB(DWS) console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.

#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane on the left, choose **Utilities** > **SQL Probes**.

#. In the probe list, click **Delete** in the **Operation** column of a probe.

   |image6|

#. Confirm the information and click **OK**.

.. _en-us_topic_0000001707293797__en-us_topic_0000001372839446_section13165101519425:

Executing a SQL Probe in One Click
----------------------------------

#. Log in to the GaussDB(DWS) console.
#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.
#. In the **Operation** column of the cluster, choose **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane on the left, choose **Utilities** > **SQL Probes**.
#. In the probe list, select a probe and click **Run**. The system will execute the selected probe and update information about the probe.
#. Confirm the information and click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001759517049.png
.. |image2| image:: /_static/images/en-us_image_0000001759357181.png
.. |image3| image:: /_static/images/en-us_image_0000001711438132.png
.. |image4| image:: /_static/images/en-us_image_0000001759517053.png
.. |image5| image:: /_static/images/en-us_image_0000001759357185.png
.. |image6| image:: /_static/images/en-us_image_0000001711438136.png
