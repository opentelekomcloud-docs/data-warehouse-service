:original_name: dws_01_72368.html

.. _dws_01_72368:

Using the Query Filter
======================

Functions
---------

When using GaussDB(DWS), performance degradation or even complete unavailability of the cluster system can occur due to slow SQL queries. To address this, GaussDB(DWS) offers a query filter function that allows you to intercept problematic statements by creating filtering rules. The filter can block slow SQL queries based on their SQL ID. Additionally, if an exception rule is triggered a certain number of times, the corresponding SQL ID is automatically added to the blacklist for interception. For details, see :ref:`Exception Rules <dws_01_72367>`. The system also supports other interception criteria, such as SQL hash and regular expression matching. Furthermore, interception rules can be customized, allowing them to be applied to specific users or databases.

.. note::

   This function is supported only by clusters of version 9.1.0.200 or later.

Creating a Query Filter
-----------------------

#. Log in to the GaussDB(DWS) console.

#. Click the name of the target cluster to access the **Basic Information** page.

#. Choose **Resource Management** and click **Query Filter**.

#. Click **Create** to add a query filter.


   .. figure:: /_static/images/en-us_image_0000002203312765.png
      :alt: **Figure 1** Adding a query filter

      **Figure 1** Adding a query filter

   .. table:: **Table 1** Parameter description

      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                    | Mandatory             | Description                                                                                                                                                                          |
      +==============================+=======================+======================================================================================================================================================================================+
      | Database Name                | Yes                   | Select a database from the drop-down list.                                                                                                                                           |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Rule Name                    | Yes                   | Name of the query filter. The value can contain 3 to 63 characters, including uppercase letters, lowercase letters, underscores (_), and dollar signs ($). The value must be unique. |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Match SQL Type               | Yes                   | You can select **SQL**, **SQL ID**, or **QL Hash**.                                                                                                                                  |
      |                              |                       |                                                                                                                                                                                      |
      |                              |                       | -  **SQL**: Query the SQL statement that matches the filtering rule.                                                                                                                 |
      |                              |                       | -  **SQL ID**: Query the **unique_sql_id** value that matches the filtering rule.                                                                                                    |
      |                              |                       | -  **SQL Hash**: Query the **sql_hash** value that matches the filtering rule.                                                                                                       |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Statement Type               | No                    | You can select **All types** (default), **SELECT**, **UPDATE**, **INSERT**, **DELETE**, and **MERGE**.                                                                               |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Bind User                    | No                    | The setting takes effect for a specified user.                                                                                                                                       |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client Name                  | No                    | Name of the application that is connected to the database. You can assign a custom application name to each connection, such as **gsql**.                                            |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client IP Address            | No                    | The setting takes effect for a specified IP address.                                                                                                                                 |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Maximum number of partitions | No                    | Estimated maximum number of partitions on the node to be scanned.                                                                                                                    |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Maximum number of tables     | No                    | Estimated maximum number of tables to be scanned.                                                                                                                                    |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Maximum Number of Rows       | No                    | Estimated maximum number of rows on a node to be scanned.                                                                                                                            |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Task Type                    | No                    | Type of a task that is actively marked.                                                                                                                                              |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Resource Pool Name           | No                    | Name of the resource pool that matches the filtering rule.                                                                                                                           |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Max. Concurrencies           | No                    | Maximum number of concurrent statements corresponding to the filtering rule.                                                                                                         |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm                        | No                    | Whether to enable alarm reporting for filtering rules.                                                                                                                               |
      +------------------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Modifying a Query Filter
------------------------

#. Log in to the GaussDB(DWS) console.

#. Click the name of the target cluster to access the **Basic Information** page.

#. Choose **Resource Management** and click **Query Filter**.

#. Click **Edit** in the **Operation** column of the target filter to modify it.

#. Confirm the information and click **OK**.


   .. figure:: /_static/images/en-us_image_0000002168066240.png
      :alt: **Figure 2** Modifying a query filter

      **Figure 2** Modifying a query filter

Deleting a Query Filter
-----------------------

#. Log in to the GaussDB(DWS) console.
#. Click the name of the target cluster to access the **Basic Information** page.
#. Choose **Resource Management** and click **Query Filter**.
#. Click **delete** in the **Operation** column of the target filter to delete it.
#. Click **OK**.
