:original_name: dws_01_72367.html

.. _dws_01_72367:

Exception Rules
===============

Feature Description
-------------------

Some complex statements may consume a large number of resources for computing, leading to performance deterioration of the entire GaussDB(DWS) database. To maintain system stability, GaussDB(DWS) allows you to customize exception rules, and terminate/downgrade the tasks that hit the rules. You can use SQL syntax to configure exception rules based on your resource and workload conditions, and associate the rules with resource pools. If there are no user-defined rules, two default exception rules will take effect.


.. figure:: /_static/images/en-us_image_0000001952008601.png
   :alt: **Figure 1** Exception rules

   **Figure 1** Exception rules

.. important::

   -  This feature is supported only by clusters of version 8.2.0 or later. The exception rule "Maximum Bandwidth on a Single DN" is supported only by clusters of version 8.2.1 or later.
   -  The cluster version 8.2.1 supports downgradation of exception rules. All exception rules support downgradation behaviors. After downgradation, only network resource preemption is downgraded to a low priority. Downgraded network queries are scheduled only when there is no normal queries.
   -  A resource pool can be associated with multiple groups of exception rules, which work in an OR way. One group of exception rules works if all its conditions are met. For example, a resource pool is associated with two groups of rules. One group specifies **elapsedtime=2400**, and the other group specifies **elapsedtime=1200** and **memsize=2000**. If the execution time of a job reaches 1200 seconds and the memory usage reaches 2,000 MB, or if the execution time reaches 2,400 seconds, the job will be terminated.
   -  Rules in the same exception rule group take effect only if all the conditions are met. For example, if you set **elapsedtime=1000** and **memsize=500**, it indicates that a job is terminated only if its execution time reaches 1,000 seconds and its memory usage reaches 500 MB. If only one of the thresholds is reached, the job will not be terminated.
   -  The default exception rules are supported only by clusters of version 8.2.0 or later. After a cluster of an earlier version is upgraded to version 8.2.0 or later, the default exception rules do not take effect. You can create exception rules as needed.
   -  The default exception rules take effect for users not associated with any resource pools, and for users whose resource pools do not have any exception rules configured. If a user-defined rule is associated with a resource pool, this rule prevails in the pool.

User-defined Exception Rules and Default Exception Rules
--------------------------------------------------------

The following table describes the user-defined exception rules and default exception rules supported by the current GaussDB (DWS) version.

.. table:: **Table 1** User-defined exception rules

   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+
   | Exception Threshold Type            | Description                                                                                                                                                                                                              | Value Range (-1 disables a parameter. **0** is not supported.) | Operation upon Exception |
   +=====================================+==========================================================================================================================================================================================================================+================================================================+==========================+
   | Blocking Time                       | Job blocking duration, in seconds. The time includes the total time spent in global and local concurrent queuing. The queuing time of each substatement (if any) in a statement is also counted.                         | -1 or 1 to INT64_MAX-1                                         | Terminate/Downgrade      |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+
   | Execution Time                      | Execution duration of a job, in seconds. The time indicates the duration from the start point of execution to the current time point. The execution time of each substatement (if any) in a statement is also counted.   | -1 or 1 to INT64_MAX-1                                         | Terminate/Downgrade      |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+
   | Total CPU time on all DNs.          | Total CPU time spent in executing a job on all DNs, in seconds.                                                                                                                                                          | -1 or 1 to INT64_MAX-1                                         | Terminate/Downgrade      |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+
   | Total CPU Time Skew Rate on All DNs | CPU time skew of a job executed on DNs. The value depends on the setting of **elapsedtime**. The system starts to check the CPU time skew of a job every 5 seconds after the job execution time reaches **elapsedtime**. | **-1**, or 1 to 100                                            | Terminate/Downgrade      |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+
   | Average CPU Usage Per DN            | Average CPU usage of a job executed across all DNs.                                                                                                                                                                      | **-1**, or 1 to 100                                            | Terminate/Downgrade      |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+
   | Data Spilled to Disk Per DN         | Allowed maximum job data spilled to disks on a DN. The unit is MB.                                                                                                                                                       | -1 or 1 to INT64_MAX-1                                         | Terminate/Downgrade      |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+
   | Maximum Bandwidth on a Single DN    | Maximum network bandwidth (MB) for a job on a single DN.                                                                                                                                                                 | -1 or 1 to INT64_MAX-1                                         | Terminate/Downgrade      |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------+--------------------------+

.. table:: **Table 2** Default exception rules

   +---------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
   | Rule Name           | Description                                                                                                                                                                                                                                                                                                        | Operation upon Exception |
   +=====================+====================================================================================================================================================================================================================================================================================================================+==========================+
   | default_cpu_percent | This rule is triggered if multiple jobs are running in a cluster, and the CPU usage of a resource pool reaches 90%. (If no resource pools are configured, the total CPU usage of the cluster is checked). This rule terminates the job whose execution time reached 15 minutes and average CPU usage exceeded 50%. | Terminate                |
   +---------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
   | default_spillsize   | This rule is triggered if the size of data spilled to disk on a single DN reaches 1/10 of the instance space during job execution in the cluster.                                                                                                                                                                  | Terminate                |
   +---------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+

Creating an Exception Rule
--------------------------

#. Log in to the GaussDB(DWS) console.
#. Click the name of the target cluster. The **Basic Information** page is displayed.
#. In the navigation pane on the left, choose **Resource Management** and switch to the **Exception Rules** tab page.
#. Click **Add** to add an exception rule.
#. Click **OK**.

   .. note::

      -  After an exception rule is created, it does not take effect immediately. You need to associate it to a resource pool. For details, see :ref:`1. Associate exception rules. <en-us_topic_0000001951848321__li1978614455413>`.
      -  The cluster version 8.2.1 supports downgradation of exception rules. All exception rules support downgradation behaviors. After downgradation, only network resource preemption is downgraded to a low priority. Downgraded network queries are scheduled only when there are no normal queries.

Editing an Exception Rule
-------------------------

#. Log in to the GaussDB(DWS) console.
#. Click the name of the target cluster. The **Basic Information** page is displayed.
#. In the navigation pane on the left, choose **Resource Management** and switch to the **Exception Rules** tab page.
#. Locate the row that contains the target exception rule and click **Edit** in the **Operation** column to edit the exception rule.

   .. note::

      -  When editing an exception rule, if you want to delete an exception rule threshold, clear the value or set it to **-1**.
      -  If the exception threshold is changed during job execution, the new threshold will take effect for the statement being executed.

Deleting an Exception Rule
--------------------------

#. Log in to the GaussDB(DWS) console.
#. Click the name of the target cluster. The **Basic Information** page is displayed.
#. In the navigation pane on the left, choose **Resource Management** and switch to the **Exception Rules** tab page.
#. Locate the row that contains the target exception rule and click **Delete** in the **Operation** column to delete the rule.

   .. note::

      If an exception rule has been associated to a resource pool, the exception rule cannot be deleted. You need to disassociate the exception rule from the resource pool before deleting it.

#. Click **OK**.
