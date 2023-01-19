:original_name: dws_01_07234.html

.. _dws_01_07234:

Modifying Workload Queues
=========================

You can modify the parameters of a workload queue.

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. In the **Workload Queue** area on the left, click the name of the queue to be modified. The following configuration areas are displayed, including **Short Query Configuration**, **Resource Configuration**, **Exception Rule**, and **Associated User**.

   |image1|

#. Modify the short query configuration. Set the parameters as required and click **Save** on the right.

   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+
   | Parameter                | Description                                                                                                                                             | Value  |
   +==========================+=========================================================================================================================================================+========+
   | Short Query Acceleration | Whether to enable short query acceleration. This function is enabled by default.                                                                        | Enable |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+
   | Concurrent Short Queries | A short query is a job whose estimated memory used for execution is less than 32 MB. The default value **-1** indicates that the job is not controlled. | 10     |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+

#. Modify the resource configuration.

   a. Click **Edit** on the right and modify the parameters according to :ref:`Table 1 <en-us_topic_0000001134560602__en-us_topic_0000001076579461_ta522b79e7bfd48449164cef2eadd83b4>`.

      .. _en-us_topic_0000001134560602__en-us_topic_0000001076579461_ta522b79e7bfd48449164cef2eadd83b4:

      .. table:: **Table 1** Workload queue parameters

         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Parameter             | Description                                                                                                                                                                       | Value                 |
         +=======================+===================================================================================================================================================================================+=======================+
         | Name                  | Name of a workload queue.                                                                                                                                                         | queue_test            |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | CPU Resource (%)      | Percentage of the CPU time slice that can be used by database users in a queue for job execution.                                                                                 | 20                    |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Memory Resource (%)   | Percentage of the memory usage by a queue.                                                                                                                                        | 20                    |
         |                       |                                                                                                                                                                                   |                       |
         |                       | .. caution::                                                                                                                                                                      |                       |
         |                       |                                                                                                                                                                                   |                       |
         |                       |    CAUTION:                                                                                                                                                                       |                       |
         |                       |    You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met. |                       |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Storage Resource (MB) | Size of the available space for permanent tables.                                                                                                                                 | 1024                  |
         |                       |                                                                                                                                                                                   |                       |
         |                       | .. caution::                                                                                                                                                                      |                       |
         |                       |                                                                                                                                                                                   |                       |
         |                       |    CAUTION:                                                                                                                                                                       |                       |
         |                       |    This parameter indicates the total tablespace of all DNs in a queue. Available space of a single DN = Configured value/Number of DNs.                                          |                       |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Query Concurrency     | Maximum number of concurrent queries in a queue.                                                                                                                                  | 10                    |
         |                       |                                                                                                                                                                                   |                       |
         |                       | .. caution::                                                                                                                                                                      |                       |
         |                       |                                                                                                                                                                                   |                       |
         |                       |    CAUTION:                                                                                                                                                                       |                       |
         |                       |    You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met. |                       |
         +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   b. Click **OK**.

#. Modify the exception rules.

   a. Modify the parameters according to :ref:`Table 2 <en-us_topic_0000001134560602__en-us_topic_0000001076579461_td5051ec72c7e48ae9c996ef77489b2db>`.

      .. note::

         Exception rules allow you to control exceptions of jobs executed by users in a queue. Currently, you can configure the parameters listed in :ref:`Table 2 <en-us_topic_0000001134560602__en-us_topic_0000001076579461_td5051ec72c7e48ae9c996ef77489b2db>`.

         -  If you select **Terminate**, you need to set the corresponding time or percentage.
         -  If you select **No restriction**, the corresponding execution rule does not take effect.

      .. _en-us_topic_0000001134560602__en-us_topic_0000001076579461_td5051ec72c7e48ae9c996ef77489b2db:

      .. table:: **Table 2** Exception rule parameters

         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Parameter                           | Description                                                                                                                                                              | Value                 |
         +=====================================+==========================================================================================================================================================================+=======================+
         | Blocking Time                       | Job blocking duration, in seconds. The duration includes the total time spent in global and local concurrent queuing.                                                    | 1200                  |
         |                                     |                                                                                                                                                                          |                       |
         |                                     | For example, if the blocking time is set to 300s, a job executed by a user in the queue will be terminated after being blocked for 300 seconds.                          |                       |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Execution Time                      | Execution duration of a job, in seconds. The time indicates the duration from the start point of execution to the current time point.                                    | 2400                  |
         |                                     |                                                                                                                                                                          |                       |
         |                                     | For example, if **Time required for execution** is set to 100s, a job executed by a user in the queue will be terminated after being executed for more than 100 seconds. |                       |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Total CPU time on all DNs           | Total CPU time spent in executing a job on all DNs, in seconds.                                                                                                          | 100                   |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Interval for Checking CPU Skew Rate | Interval for checking the CPU skew, in seconds. This parameter must be set together with **Total CPU Time on All DNs**.                                                  | 2400                  |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Total CPU Time Skew Rate on All DNs | CPU time skew rate of a job executed on DNs. The value depends on the setting of **Interval for Checking CPU Skew Rate**.                                                | 90                    |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   b. Click **Save**.

#. Associate users.

   .. note::

      -  The resources used by a user to run jobs can be controlled only after the user is added to a queue.
      -  A database user can be added to only one queue. Users removed from a queue can be added to another queue.
      -  The administrator cannot be associated.

   a. Click **Add** on the right.

   b. Select the users to be added from the current user list. You can select multiple users at a time.

      |image2|

   c. Click **OK**.

   d. To delete a user, click **Delete** in the **Operation** column of the user.

.. |image1| image:: /_static/images/en-us_image_0000001134560814.png
.. |image2| image:: /_static/images/en-us_image_0000001180320461.png
