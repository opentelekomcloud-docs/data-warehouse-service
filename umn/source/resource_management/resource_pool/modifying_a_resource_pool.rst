:original_name: dws_01_07234.html

.. _dws_01_07234:

Modifying a Resource Pool
=========================

You can modify the parameters of a resource pool on the resource management page.

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. In the **Resource Pools** drop-down list, click the name of a resource pool. The following configuration areas are displayed, including **Short Query Configuration**, **Resource Configuration**, **Exception Rule**, and **Associated User**.

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

   a. Click **Edit** on the right and modify the parameters. For more information, see :ref:`Resource pool parameters <en-us_topic_0000001466594826__table1380933134011>`.

      .. _en-us_topic_0000001466594826__table1380933134011:

      .. table:: **Table 1** Resource pool parameters

         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Parameter             | Description                                                                                                                                                                                                                                                  | Example Value         |
         +=======================+==============================================================================================================================================================================================================================================================+=======================+
         | Name                  | Resource pool name                                                                                                                                                                                                                                           | pool_1                |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | CPU Resource (%)      | -  CPU share: Percentage of CPU time that can be used by users associated with the current resource pool to execute jobs. The value is an integer ranging from 1 to 99.                                                                                      | 30                    |
         |                       | -  CPU limit: Maximum percentage of CPU cores used by a database user in a resource pool. The value is an integer ranging from 0 to 100. **0** indicates no limit.                                                                                           |                       |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       | .. note::                                                                                                                                                                                                                                                    |                       |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       |    -  The sum of the parameter values of all the resource pools cannot exceed 99%. If there is only one resource pool, the CPU share parameter does not take effect.                                                                                         |                       |
         |                       |    -  The CPU share parameter takes effect only when CPU contention occurs. For example, resource pools A and B are bound to CPU 1. If A and B are both running, the parameter takes effect. If there is only A running, the parameter does not take effect. |                       |
         |                       |    -  The sum of the CPU limits of all the resource pools cannot exceed 100%. The default value is 0.                                                                                                                                                        |                       |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Memory Resource (%)   | Percentage of the memory that can be used by a resource pool.                                                                                                                                                                                                | 20                    |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       | .. caution::                                                                                                                                                                                                                                                 |                       |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       |    CAUTION:                                                                                                                                                                                                                                                  |                       |
         |                       |    You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.                                                                            |                       |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Storage Resource (MB) | Size of the available space for permanent tables.                                                                                                                                                                                                            | 1024                  |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       | .. caution::                                                                                                                                                                                                                                                 |                       |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       |    CAUTION:                                                                                                                                                                                                                                                  |                       |
         |                       |    This parameter indicates the total tablespace of all DNs in a resource pool. Available space of a single DN = Configured value/Number of DNs.                                                                                                             |                       |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Query Concurrency     | Maximum number of concurrent queries in a resource pool.                                                                                                                                                                                                     | 10                    |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       | .. caution::                                                                                                                                                                                                                                                 |                       |
         |                       |                                                                                                                                                                                                                                                              |                       |
         |                       |    CAUTION:                                                                                                                                                                                                                                                  |                       |
         |                       |    You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.                                                                            |                       |
         +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

      .. note::

         Only 8.1.3 and later versions support the CPU limit management.

   b. Click **OK**.

#. Modify the exception rules.

   a. Modify rule parameters. See the following table for more information.

      .. _en-us_topic_0000001466594826__table450693015419:

      .. table:: **Table 2** Exception rule parameters

         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
         | Parameter                           | Description                                                                                                                                                                                                                    | Value Range (0 Means No Limit)                                                  | Operation                         |
         +=====================================+================================================================================================================================================================================================================================+=================================================================================+===================================+
         | Blocking Time                       | Job blocking time. It refers to the total time spent in global and local concurrent queuing. The unit is second.                                                                                                               | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
         |                                     |                                                                                                                                                                                                                                |                                                                                 |                                   |
         |                                     | For example, if the blocking time is set to 300s, a job executed by a user in the resource pool will be terminated after being blocked for 300 seconds.                                                                        |                                                                                 |                                   |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
         | Execution Time                      | Time that has been spent in executing the job, in seconds.                                                                                                                                                                     | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
         |                                     |                                                                                                                                                                                                                                |                                                                                 |                                   |
         |                                     | For example, if **Time required for execution** is set to 100s, a job executed by a user in the resource pool will be terminated after being executed for more than 100 seconds.                                               |                                                                                 |                                   |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
         | Total CPU time on all DNs.          | Total CPU time spent in executing a job on all DNs, in seconds.                                                                                                                                                                | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
         | Interval for Checking CPU Skew Rate | Interval for checking the CPU skew, in seconds. This parameter must be set together with **Total CPU Time on All DNs**.                                                                                                        | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
         | Total CPU Time Skew Rate on All DNs | CPU time skew rate of a job executed on DNs. The value depends on the setting of **Interval for Checking CPU Skew Rate**.                                                                                                      | An integer in the range 1 to 100. The value **0** indicates no limit.           | **Terminated** or **Not limited** |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
         | Data Spilled to Disk Per DN         | Allowed maximum job data spilled to disks on a DN. The unit is MB.                                                                                                                                                             | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
         | Average CPU Usage Per DN            | Average CPU usage of a job on each DN. If **Interval for Checking CPU Skew Rate** is configured, the interval takes effect for this parameter. If the interval is not configured, the check interval is 30 seconds by default. | An integer in the range 1 to 100. The value **0** indicates no limit.           | **Terminated** or **Not limited** |
         +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+

      .. note::

         Exception rules allow you to control exceptions of jobs executed by users in a resource pool. Currently, you can configure the parameters listed in :ref:`Table 2 <en-us_topic_0000001466594826__table450693015419>`.

         -  If you select **Terminate**, you need to set the corresponding time or percentage.
         -  If you select **No restriction**, the corresponding execution rule does not take effect.

   b. Click **Save**.

#. Associate users.

   .. note::

      -  The resources used by a user to run jobs can be controlled only after the user is added to a resource pool.
      -  A database user can be added to only one resource pool. Users removed from a resource pool can be added to another pool.
      -  Database administrators cannot be associated.

   a. Click **Add**.

   b. Select the users to be added from the current user list. You can select multiple users at a time.

      |image2|

   c. Click **OK**.

   d. To remove a user, click **Disassociate User** in the **Operation** column of the user.

.. |image1| image:: /_static/images/en-us_image_0000001466754646.png
.. |image2| image:: /_static/images/en-us_image_0000001466594994.png
