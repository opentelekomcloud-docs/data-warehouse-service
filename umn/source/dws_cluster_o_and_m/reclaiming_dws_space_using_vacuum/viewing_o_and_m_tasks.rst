:original_name: dws_01_1163.html

.. _dws_01_1163:

Viewing O&M Tasks
=================

#. Log in to the DWS console.
#. Click the name of the target cluster.
#. In the navigation pane, choose **Intelligent O&M**.
#. Switch to the **O&M Status** area.
#. Check that the following tables are sorted in descending order based on the size of released space: tables being vacuumed, tables to be vacuumed, tables that have been vacuumed, and tables that failed to be vacuumed. A maximum of 100 tables can be displayed on each page.

   -  If more than 100 tables are processed by an O&M task, you are advised to create multiple O&M tasks and shorten the scheduling period of each task to handle fewer tables at a time.

   -  If there are still more than 100 tables after multiple O&M tasks are created, you can run the command below to query the **scheduler.vacuum_full_rslt** table in the cluster database **postgres**. The **vacuum_full_rslt** system catalog stores automatic vacuum information and can only be queried. For details about the fields in the table, see :ref:`Table 1 <en-us_topic_0000002270493705__table7828124612116>`.

      .. code-block::

         SELECT * FROM scheduler.vacuum_full_rslt
         WHERE Mission_Start_Time = 'xxxx-xx-xx xx:xx:xx' AND
               Mission_End_Time = 'xxxx-xx-xx xx:xx:xx'
         order by spacerelease desc;

      .. _en-us_topic_0000002270493705__table7828124612116:

      .. table:: **Table 1** vacuum_full_rslt information

         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | Field                    | Description                                                                                                             |
         +==========================+=========================================================================================================================+
         | databasename             | Database name.                                                                                                          |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | schemaname               | Schema name.                                                                                                            |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | tablename                | Table name.                                                                                                             |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | tableclass               | Table class.                                                                                                            |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | vacuumstatus             | Vacuum status.                                                                                                          |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | expandedratebeforevacuum | Expansion rate before vacuum.                                                                                           |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | expandedrateaftervacuum  | Expansion rate after vacuum.                                                                                            |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | retrievingspace          | Estimated space that can be reclaimed before vacuum.                                                                    |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | spacerelease             | Difference between the tablespaces before and after vacuum. The value is affected by the data imported to the database. |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | categoryid               | ID of the vacuum O&M task.                                                                                              |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | mission_start_time       | Start time of the vacuum task.                                                                                          |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | mission_end_time         | End time of the vacuum task.                                                                                            |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | lastvacuumtime           | Time when the record is imported to the database.                                                                       |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | tablesizebefore          | Table size before vacuum.                                                                                               |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | tablesizeafter           | Table size after vacuum.                                                                                                |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | smallcuratebeforevacuum  | Percentage of small CUs before vacuum.                                                                                  |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | smallcurateaftervacuum   | Percentage of small CUs after vacuum.                                                                                   |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | failedreason             | Vacuum failure cause.                                                                                                   |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | taskstarttime            | Start time of the vacuum task.                                                                                          |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+
         | taskendtime              | End time of the vacuum task.                                                                                            |
         +--------------------------+-------------------------------------------------------------------------------------------------------------------------+

   -  If the cluster is read-only, the INSERT statement cannot be executed for intelligent O&M tasks. There may be tasks remaining in the **Running** status. The **Running** status in this case is a historical status, and it indicates that the task is not completed within the specified time. If you manually pause the task and the task is not scheduled, the task may remain in the **Waiting** status. In this case, cancel the cluster read-only state and contact technical support to update the task status.
