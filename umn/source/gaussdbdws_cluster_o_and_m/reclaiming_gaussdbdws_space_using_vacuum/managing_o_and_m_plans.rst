:original_name: dws_01_1162.html

.. _dws_01_1162:

Managing O&M Plans
==================

Prerequisites
-------------

-  Avoid setting the time window for automatic Vacuum O&M tasks during peak hours to prevent conflicts with user services.
-  For user tables, the maximum concurrency for Vacuum Full O&M tasks is 24, and the minimum is 0. For system tables, the maximum concurrency is 1, and the minimum is 0. The concurrency value adjusts automatically based on **io_util**.

   -  Two intervals for 0% to 60%

      -  0% to 30%: The concurrency value increases by 2 each time the value of **io_util** decreases by 15%.
      -  30% to 60%: The concurrency value is incremented by 1 each time the value of **io_util** decreases by 15%.

   -  60% to 70%: The concurrency value remains unchanged.
   -  Above 70%: The concurrency value decreases by 1 until it reaches 0.

-  The scheduler scans the expansion of column-store compression units (CUs) within the time window. If the average number of CU records in a column-store table is less than 1000, the scheduler scans the table first. The scanning of column-store CUs is not limited by table bloat or table reclaimable space.
-  A maximum of 100 tables can be added to the priority list.
-  The scheduler autovacuum function depends on the statistics. If the statistics are inaccurate, the execution sequence and results may be affected.
-  The scheduler does not support names containing spaces or single quotation marks, including database names, schema names, and table names. Otherwise, the tables will be skipped. Tables with spaces or single quotes in the priority list are skipped automatically.

Setting the Common Configurations of O&M Tasks
----------------------------------------------

**Precautions**

-  The configuration applies to the VacuumFull O&M task for each user table. Running tasks are unaffected. The new periodic task settings take effect on the next run.

**Procedure**

#. Log in to the GaussDB(DWS) console.
#. Click the name of the target cluster.
#. In the navigation pane, choose **Intelligent O&M**.
#. In the **Common O&M Task Configurations** area on the upper part of the page, modify the following common configuration items:

   -  **Maximum number of concurrent O&M tasks in the VacuumFull user table**: applies to VacuumFull O&M tasks for each user table. You are advised to set this parameter based on the available disk space and I/O load within a specific time window. The value range is 1 to 24. Configure it based on the remaining disk space and I/O load. You are advised to set it to 5. Running tasks are not affected. For periodic tasks, the new limit for concurrent tasks applies the next time the O&M task runs.
   -  Small CU threshold: helps identify small CU tables. A table is classified as small if its CU value is lower than the threshold. When the CU value is at or below the threshold, it triggers Vacuum. A higher threshold value increases the trigger sensitivity. The recommended default value for this parameter is **1000**.
   -  Small CU ratio: indicates the ratio of small CU tables to all CU tables. When the ratio is at or above the parameter value, Vacuum is triggered. A lower value increases the trigger sensitivity. The recommended default value for this parameter is **50%**.

#. Confirm the settings and click **Save**.

.. _en-us_topic_0000002203312277__section12256103112263:

Adding an O&M Plan
------------------

#. Log in to the GaussDB(DWS) console.

#. Click the name of the target cluster.

#. In the navigation pane, choose **Intelligent O&M**.

#. In the **O&M Plan** area, click **Add O&M Task**.

#. In the displayed **Add O&M Task** dialog box, configure basic information about the O&M task.

   .. table:: **Table 1** Basic configuration items of an O&M task

      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
      | Configuration Item    | Description                                                                                                                                                                                                                                                                                                                                                                                                | Example                                                                                     |
      +=======================+============================================================================================================================================================================================================================================================================================================================================================================================================+=============================================================================================+
      | O&M Task              | Vacuum (Currently, only Vacuum O&M tasks are supported.)                                                                                                                                                                                                                                                                                                                                                   | Vacuum                                                                                      |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
      | Description           | Brief description of the intelligent O&M task.                                                                                                                                                                                                                                                                                                                                                             | This intelligent O&M task helps users periodically invoke Vacuum commands to reclaim space. |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
      | Remarks               | Supplementary information.                                                                                                                                                                                                                                                                                                                                                                                 | ``-``                                                                                       |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
      | Scheduling Mode       | The supports the following scheduling modes:                                                                                                                                                                                                                                                                                                                                                               | Specify                                                                                     |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       | -  **Auto**: Intelligent O&M scans the database in a specified time window, and automatically delivers table-level vacuum tasks by service load and reclaimable space of user tables.                                                                                                                                                                                                                      |                                                                                             |
      |                       | -  **Specify**: You need to specify a vacuum target. Intelligent O&M will automatically deliver a table-level vacuum task in a specified time window.                                                                                                                                                                                                                                                      |                                                                                             |
      |                       | -  **Priority**: You can specify the preferential vacuum targets. During the remaining time window (if any), Intelligent O&M will automatically scan other tables that can be vacuumed and deliver table-level vacuum tasks.                                                                                                                                                                               |                                                                                             |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                             |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       |    You are advised to select **Specify** for **VACUUM** and **VACUUM FULL** operations. Do not perform **VACUUM FULL** on wide column-store tables. Otherwise, memory bloat may occur.                                                                                                                                                                                                                     |                                                                                             |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
      | Autovacuum            | Supported: system catalog Vacuum or user table VacuumFull.                                                                                                                                                                                                                                                                                                                                                 | User table (VACUUM FULL)                                                                    |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       | -  A system catalog VACUUM transaction holds a level-4 lock (share update exclusive lock), which does not affect user services. Only the transactions on the DDL process of the system catalog are blocked.                                                                                                                                                                                                |                                                                                             |
      |                       | -  A user table VACUUM FULL transaction holds a level-8 lock (access exclusive lock). All the other transactions on the table are blocked until VACUUM FULL is complete. To avoid affecting services, you are advised to perform VACUUM FULL during off-peak hours.                                                                                                                                        |                                                                                             |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       |    .. caution::                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       |       CAUTION:                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                             |
      |                       |       During **VACUUM FULL**, the space usage will first increase and then decrease, because this operation requires the same space as the table to be vacuumed. The actual table size is calculated as the total table size multiplied by (1 - dirty page rate). Ensure you have sufficient space before doing **VACUUM FULL**.                                                                           |                                                                                             |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
      | Vacuum First          | You can configure the preferred Vacuum target. Each row corresponds to a table. Each table is represented by the database name, schema name, and table name, which are separated by spaces.                                                                                                                                                                                                                | ``-``                                                                                       |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+
      | Advanced settings     | If you select **Custom**, you can configure the autovacuum triggers, including the table bloat and table reclaimable space. If you select **Default**, the default configuration is used.                                                                                                                                                                                                                  | Default configuration (table bloat rate 80% or reclaimable space 100 GB.)                   |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                             |
      |                       |                                                                                                                                                                                                                                                                                                                                                                                                            |                                                                                             |
      |                       |    VACUUM bloat rate: After frequent UPDATE and DELETE operations are performed in a database, the deleted or updated rows are logically deleted from the database, but actually still exist in tables. Before VACUUM is complete, such data is still stored in disks, causing table bloat. If the bloat rate reaches the percentage threshold set in an O&M task, VACUUM will be automatically triggered. |                                                                                             |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------+

#. Click **Next** > **Schedule** to configure scheduling for O&M tasks.

   Select an O&M type.

   -  **One-off**: Set the start time and end time of the task.
   -  **Periodic**: Select a time window type, which includes **Daily**, **Weekly**, and **Monthly**, and select a time segment. Intelligent O&M will automatically analyze the time window and deliver O&M tasks accordingly.

#. Click **Next: Finish**. After you confirm the information, click **Finish** to submit the request.

Modifying an O&M Plan
---------------------

#. Log in to the GaussDB(DWS) console.

#. Click the name of the target cluster.

#. In the navigation pane, choose **Intelligent O&M**.

#. In the **O&M Plan** area, click **Modify** in the **Operation** column of the target task.

   |image1|

#. The **Modify O&M Task** page is displayed. The parameters for modifying an O&M task are identical to those for adding one. For details, see :ref:`Adding an O&M Plan <en-us_topic_0000002203312277__section12256103112263>`. The modification takes effect upon the next running.

#. Confirm the modification and click **OK**.

Viewing O&M Task Details
------------------------

#. Log in to the GaussDB(DWS) console.

#. Click the name of the target cluster.

#. In the navigation pane, choose **Intelligent O&M**.

#. In the **O&M Plan** area, click **Details** in the **Operation** column of the target task.

   |image2|

#. The **O&M Task Details** panel is displayed for you to check the information.

.. |image1| image:: /_static/images/en-us_image_0000002203312797.png
.. |image2| image:: /_static/images/en-us_image_0000002168066276.png
