:original_name: dws_01_1332.html

.. _dws_01_1332:

Viewing DWS Database Details
============================

The **Database Monitoring** page displays the real-time and historical resource consumption of a database.

Database Monitoring
-------------------

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.
#. In the navigation pane on the left, choose **Monitoring** > **Database Monitoring**.

Database Resource Consumption
-----------------------------

You can select a database to view its resource usage and performance metrics. For details about the metrics, see :ref:`Table 1 <en-us_topic_0000002329649378__en-us_topic_0000001372999354_table138195983612>`.

.. _en-us_topic_0000002329649378__en-us_topic_0000001372999354_table138195983612:

.. table:: **Table 1** Database resource consumption metrics

   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Metric Name             | Description                                                                                                        | Monitoring Interval (Raw Data) |
   +=========================+====================================================================================================================+================================+
   | Database Name           | Name of the database                                                                                               | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Usage (GB)              | Used capacity of the database                                                                                      | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Monitoring              | You can click the monitoring icon to view the number of queries, number of sessions, and capacity of the database. | ``-``                          |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Users                   | Number of users in the database                                                                                    | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Applications            | Number of applications in the database                                                                             | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Sessions                | Number of database sessions                                                                                        | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Queries                 | Number of queries in the database                                                                                  | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Number of Inserted Rows | Number of rows inserted by queries in this database                                                                | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Number of Updated Rows  | Number of rows updated by queries in this database                                                                 | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Number of Deleted Rows  | Number of rows deleted by queries in this database                                                                 | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Deadlocks               | Number of deadlocks detected in this database                                                                      | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Temporary Files         | Number of temporary files created by queries in this database.                                                     | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Temporary File Capacity | Total amount of data written to temporary files by queries in this database.                                       | 180s                           |
   +-------------------------+--------------------------------------------------------------------------------------------------------------------+--------------------------------+
