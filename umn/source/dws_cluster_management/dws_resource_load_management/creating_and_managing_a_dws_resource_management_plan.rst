:original_name: dws_01_72365.html

.. _dws_01_72365:

Creating and Managing a DWS Resource Management Plan
====================================================

Functions
---------

The resource management plan is an advanced resource management feature provided by DWS. You can create a resource management plan, add multiple stages to the plan, and configure different queue resource ratios for the stages. After a plan is started, it automatically changes the resource configurations in different stages as scheduled. If you need to run services in different stages with different proportions of resources, you can create a resource management plan to automatically change resource configurations in different stages.

Notes and Constraints
---------------------

-  The default start time is the UTC time. The next execution time is your local time. Configure the time, date, and month. Do not set an invalid date, for example, February 30.
-  Stages cannot be added to or deleted from a running resource management plan.
-  Before creating a resource management plan and importing stages, you must design and create a resource pool. For details, see :ref:`Creating a Resource Pool <en-us_topic_0000002344532677__en-us_topic_0000001372520138_section1239585215495>`.
-  You can create up to 10 resource management plans, but only one plan can be started for each cluster. Each plan can contain up to 48 stages, and must have at least two stages before it can be started.
-  You cannot import or delete a running resource management plan.

Adding a Resource Management Plan Stage
---------------------------------------

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. Click the **Resource Management Plan** tab, click **Add** in the **Stage** area, and set the parameters listed below.

   .. _en-us_topic_0000002310493842__en-us_topic_0000001372679718_table129717370819:

   .. table:: **Table 1** Parameters for adding a stage

      +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter               | Description                                                                                                                                            | Default Value         |
      +=========================+========================================================================================================================================================+=======================+
      | Stage                   | Stage name. The name must start with a lowercase letter and contain 3 to 28 characters, including only lowercase letters, digits, and underscores (_). | dws_demo              |
      +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Month                   | Select the execution months of the stage. You can select **All**.                                                                                      | All                   |
      +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Day                     | Select a date or dates. You can select **All**.                                                                                                        | All                   |
      |                         |                                                                                                                                                        |                       |
      |                         | Exercise caution when you select dates 29th, 30th, and 31st, because they do not occur in all months.                                                  |                       |
      +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Start Time              | Start time of the plan phase. The UTC time is used by default. Set the policy based on the time zone.                                                  | ``-``                 |
      +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Selected Resource Pools | You can select the resource pool for implementing the plan stage.                                                                                      | ``-``                 |
      +-------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

Managing Stages of a Resource Management Plan
---------------------------------------------

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. Click the **Resource Management Plan** tab, and you can modify, switch, view, import, export, and delete the stages of a resource management plan. :ref:`Table 2 Operations on the stages of a resource management plan <en-us_topic_0000002310493842__en-us_topic_0000001372679718_table1241553816363>` lists the operations.

   .. _en-us_topic_0000002310493842__en-us_topic_0000001372679718_table1241553816363:

   .. table:: **Table 2** Operations on the stages of a resource management plan

      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Operation | Description                                                                                                                                                                                                                                                     |
      +===========+=================================================================================================================================================================================================================================================================+
      | Modify    | Click **Modify** in the **Operation** column of the plan stage. Modify parameters, such as the stage changing time and resource configurations. For details, see :ref:`Table 1 <en-us_topic_0000002310493842__en-us_topic_0000001372679718_table129717370819>`. |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Switch    | Click the switch button in the plan overview area, and select the target stage. The switchover time of all stages in a plan cannot be the same.                                                                                                                 |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View      | Click **View** in the **Operation** column of the plan stage to view the switchover time of the current stage.                                                                                                                                                  |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Import    | In the plan stages area, click **Import** to import a stage of the resource management plan.                                                                                                                                                                    |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Export    | In the plan stages area, click **Export** to export a stage of the resource management plan.                                                                                                                                                                    |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Delete    | Click **Delete** in the **Operation** column of the plan stage to delete the stage.                                                                                                                                                                             |
      +-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Creating a Resource Management Plan
-----------------------------------

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. Click the **Resource Management Plans** tab and click **Add**.

   Enter the name of the resource management plan. Stage name. The name must start with a lowercase letter and contain 3 to 28 characters, including only lowercase letters, digits, and underscores (_).

#. Click **OK**.

Managing a Resource Management Plan
-----------------------------------

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. Click the **Resource Management Plans** tab. Then you can start, stop, delete, and view execution logs of a resource management plan. :ref:`Table 3 Operations on a resource management plan <en-us_topic_0000002310493842__en-us_topic_0000001372679718_table95484112376>` lists the operations.

   .. _en-us_topic_0000002310493842__en-us_topic_0000001372679718_table95484112376:

   .. table:: **Table 3** Operations on a resource management plan

      +-----------+-------------------------------------------------------------------------------------------+
      | Operation | Description                                                                               |
      +===========+===========================================================================================+
      | Start     | Click **Start** in the plan overview area to start the resource management plan.          |
      +-----------+-------------------------------------------------------------------------------------------+
      | Stop      | Click **Stop** in the plan overview area to stop the resource management plan.            |
      +-----------+-------------------------------------------------------------------------------------------+
      | View      | Click **View** in the plan execution logs area to view execution logs of the plan stages. |
      +-----------+-------------------------------------------------------------------------------------------+
      | Delete    | Click **Delete** in the plan overview area to delete the current plan.                    |
      +-----------+-------------------------------------------------------------------------------------------+

#. Click **OK**.
