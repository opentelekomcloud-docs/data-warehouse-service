:original_name: dws_01_72368.html

.. _dws_01_72368:

Using DWS Query Filters to Intercept Slow SQL Statements
========================================================

Functions
---------

When using DWS, performance degradation or even complete unavailability of the cluster system can occur due to slow SQL queries. To address this, DWS offers a query filter function that allows you to intercept problematic statements by creating filtering rules.

The filter can block slow SQL queries based on their SQL ID. Additionally, if an exception rule is triggered a certain number of times, the corresponding SQL ID is automatically added to the blacklist for interception. For details, see :ref:`exception rules <dws_01_72367>`. The query filter also supports more interception rules, such as SQL hash and regular expression matching. Interception rules can be associated with a specific user or database.

Application Scenarios
---------------------

The main application scenarios are as follows:

-  Circuit breaker mechanism: After exception rules are enabled, queries that frequently trigger exception rules will be automatically added to the list for interception.
-  Emergent termination: If a job causes a core dump, hang, or severe performance, you can add it to the list to immediately terminate it.

Notes and Constraints
---------------------

The query filter is supported only by clusters of version 9.1.0.200 or later.

Creating a Query Filter
-----------------------

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. Click the **Query Filter** tab. On the displayed page, click **Create** and set the parameters listed in :ref:`Table 1 <en-us_topic_0000002344532681__en-us_topic_0000002105411190_table13664645155414>`.

   .. _en-us_topic_0000002344532681__en-us_topic_0000002105411190_table13664645155414:

   .. table:: **Table 1** Parameter description

      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                    | Mandatory             | Description                                                                                                                                                                                                                                                        |
      +==============================+=======================+====================================================================================================================================================================================================================================================================+
      | Database Name                | Yes                   | Select a database from the drop-down list.                                                                                                                                                                                                                         |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Rule Name                    | Yes                   | Name of the query filter. The value can contain 3 to 63 characters, including uppercase letters, lowercase letters, underscores (_), and dollar signs ($). The value must be unique.                                                                               |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Match SQL Type               | Yes                   | You can select **SQL**, **SQL ID**, or **SQL Hash**.                                                                                                                                                                                                               |
      |                              |                       |                                                                                                                                                                                                                                                                    |
      |                              |                       | -  **SQL**: Query the SQL statement that matches the filtering rule.                                                                                                                                                                                               |
      |                              |                       | -  **SQL ID**: Query the **unique_sql_id** value that matches the filtering rule. To obtain the value, query the **unique_sql_id** field in the **GS_WLM_SESSION_INFO** view.                                                                                      |
      |                              |                       | -  **SQL Hash**: Query the **sql_hash** value that matches the filtering rule. To obtain the value, query the **sql_hash** field in the **GS_WLM_SESSION_INFO** view.                                                                                              |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Statement Type               | No                    | You can select **All types** (default), **SELECT**, **UPDATE**, **INSERT**, **DELETE**, and **MERGE**.                                                                                                                                                             |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Bind User                    | No                    | The setting takes effect for a specified user.                                                                                                                                                                                                                     |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client Name                  | No                    | Name of the application that is connected to the database. You can assign a custom application name to each connection, such as **gsql**.                                                                                                                          |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client IP Address            | No                    | The setting takes effect for a specified IP address.                                                                                                                                                                                                               |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Maximum number of partitions | No                    | Estimated maximum number of partitions on the node to be scanned.                                                                                                                                                                                                  |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Maximum number of tables     | No                    | Estimated maximum number of tables to be scanned.                                                                                                                                                                                                                  |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Maximum Number of Rows       | No                    | Estimated maximum number of rows on a node to be scanned.                                                                                                                                                                                                          |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Task Type                    | No                    | Task type that is actively identified. It is marked by the **query_band** parameter. For details, see "Query Band Load Identification" in the *Data Warehouse Service (DWS) Developer Guide*.                                                                      |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Resource Pool Name           | No                    | Name of the resource pool that matches the filtering rule.                                                                                                                                                                                                         |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Max. Concurrencies           | No                    | Maximum number of concurrent statements corresponding to the filtering rule.                                                                                                                                                                                       |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm                        | No                    | Controls whether an alarm or error is reported when a statement is intercepted. If this option is disabled, an error is reported when the SQL statement meets the filter criteria. If this option is enabled, no error is reported, and only an alarm is reported. |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Query Filter Management Operations
----------------------------------

#. Log in to the DWS console.
#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.
#. In the navigation pane, choose **Resource Management**.
#. Click the **Query Filter** tab. You can edit or delete query filters. The following table lists the operations.

   .. table:: **Table 2** Query filter management operations

      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operation | Description                                                                                                                                                                                                     |
      +===========+=================================================================================================================================================================================================================+
      | Edit      | Locate a query filter and click **Edit** in the **Operation** column to modify the filter parameters listed in :ref:`Table 1 <en-us_topic_0000002344532681__en-us_topic_0000002105411190_table13664645155414>`. |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Delete    | Locate a query filter and click **Delete** in the **Operation** column to delete the filter.                                                                                                                    |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Examples
--------

This example describes how to create a filter to block the **dbadmin** user's queries on the **test_table** table marked by **test_bond** through the **test_application** client.

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. On the **Query Filters** page, click **Create Query Filter**. Set the following parameters (and keep the default values for other parameters).

   -  **Database Name**: Select **gaussdb**.

   -  **Rule Name**: Enter **test_block**.

   -  **SQL**: Enter **SELECT \\*FROM test_table**.

   -  **Statement Type**: Select **SELECT**.

   -  **Bound User**: Select **dbadmin**.

   -  **Client Name**: Enter **test_application**.

   -  **Task Type**: Enter **test_band**.


      .. figure:: /_static/images/en-us_image_0000002548462889.png
         :alt: **Figure 1** Creating a filter

         **Figure 1** Creating a filter

#. Click **OK**. The filter is successfully created and the cluster list is displayed.

#. In the navigation pane on the left, choose **Data** > **SQL Editor**. After the data source is successfully connected, run the following SQL statements to create the data source table **test_table** on the editing panel:

   .. code-block::

      CREATE TABLE test_table(a int,b int);
      INSERT INTO test_table(a,b) SELECT generate_series(1,100), generate_series(1,100);


   .. figure:: /_static/images/en-us_image_0000002548582895.png
      :alt: **Figure 2** Creating a data source table

      **Figure 2** Creating a data source table

#. Check whether the query that meets the conditions is blocked. If yes, the query filter takes effect.

   .. code-block::

      SET query_band='test_band';
      SET application_name='test_application';
      SELECT * FROM test_table WHERE a < 2;

#. To remove the **test_block** rule, return to the **Query Filters** page and click **Delete** in the **Operation** column of the rule.
