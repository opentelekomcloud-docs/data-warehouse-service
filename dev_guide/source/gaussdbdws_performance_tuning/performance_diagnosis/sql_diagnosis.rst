:original_name: dws_04_0491.html

.. _dws_04_0491:

SQL Diagnosis
=============

GaussDB(DWS) clusters support SQL diagnosis, which shows the complete execution plans of specific SQL queries. You can search for SQL queries (such as slow queries) using a combination of multiple filter criteria.

To use SQL diagnosis, perform the following steps:

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**.
#. In the navigation pane on the left, choose **Utilities** > **SQL Diagnosis**. The metrics include:

   -  Query ID
   -  Database
   -  Schema Name
   -  User Name
   -  Client
   -  Client IP Address
   -  Running Time (ms)
   -  CPU Time (ms)
   -  Scale-Out Started
   -  Completed
   -  Details

#. On the **SQL Diagnosis** page, you can view the SQL diagnosis information. In the **Details** column of a specified query ID, click **View** to view the detailed SQL diagnosis result, including:

   -  Alarm Information
   -  SQL Statement
   -  Execution Plan
