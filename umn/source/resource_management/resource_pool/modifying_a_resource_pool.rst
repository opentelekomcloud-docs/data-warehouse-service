:original_name: dws_01_07234.html

.. _dws_01_07234:

Modifying a Resource Pool
=========================

You can modify the parameters of a resource pool on the resource management page.

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. In the **Resource Pools** drop-down list, click the name of a resource pool. The following configuration areas are displayed, including **Short Query Configuration**, **Resource Configuration**, **Exception Rule**, and **Associated User**.

#. Modify the short query configuration. Set the parameters as required and click **Save** on the right.

   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+
   | Parameter                | Description                                                                                                                                             | Value  |
   +==========================+=========================================================================================================================================================+========+
   | Short Query Acceleration | Whether to enable short query acceleration. This function is enabled by default.                                                                        | Enable |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+
   | Concurrent Short Queries | A short query is a job whose estimated memory used for execution is less than 32 MB. The default value **-1** indicates that the job is not controlled. | 10     |
   +--------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+

#. Modify the resource configuration.

   a. Click **Edit** on the right and modify the parameters according to :ref:`Table 1 <en-us_topic_0000001951848321__table1380933134011>`.

      .. _en-us_topic_0000001951848321__table1380933134011:

      .. table:: **Table 1** Resource pool parameters

         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Parameter                     | Description                                                                                                                                                                                                                                                  | Default Value         |
         +===============================+==============================================================================================================================================================================================================================================================+=======================+
         | Name                          | Resource pool name                                                                                                                                                                                                                                           | ``-``                 |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | CPU Resource (%)              | -  CPU share: Percentage of CPU time that can be used by users associated with the current resource pool to execute jobs. The value is an integer ranging from 1 to 99.                                                                                      | ``-``                 |
         |                               | -  CPU limit: Maximum percentage of CPU cores used by a database user in a resource pool. The value is an integer ranging from 0 to 100. **0** indicates no limit.                                                                                           |                       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | .. note::                                                                                                                                                                                                                                                    |                       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               |    -  The sum of the parameter values of all the resource pools cannot exceed 99%. If there is only one resource pool, the CPU share parameter does not take effect.                                                                                         |                       |
         |                               |    -  The CPU share parameter takes effect only when CPU contention occurs. For example, resource pools A and B are bound to CPU 1. If A and B are both running, the parameter takes effect. If there is only A running, the parameter does not take effect. |                       |
         |                               |    -  The sum of the CPU limits of all the resource pools cannot exceed 100%. The default value is 0.                                                                                                                                                        |                       |
         |                               |    -  The CPU limit is supported only by clusters of version 8.1.3 or later.                                                                                                                                                                                 |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Memory Resource (%)           | Percentage of the memory that can be used by a resource pool.                                                                                                                                                                                                | 0 (not limited)       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.                                                                               |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Storage Resource (MB)         | Size of the available space for permanent tables.                                                                                                                                                                                                            | -1 (not limited)      |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | This parameter indicates the total tablespace of all DNs in a resource pool. Available space of a single DN = Configured value/Number of DNs.                                                                                                                |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Complex Statement Concurrency | Maximum number of concurrent queries in a resource pool.                                                                                                                                                                                                     | 10                    |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.                                                                               |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Network Bandwidth Weight      | Weight for network scheduling. The value is an integer ranging from 1 to 2147483647. The default value is -1.                                                                                                                                                | -1 (not limited)      |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | .. caution::                                                                                                                                                                                                                                                 |                       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               |    CAUTION:                                                                                                                                                                                                                                                  |                       |
         |                               |    Only cluster 8.2.1 and later versions support the network bandwidth weight.                                                                                                                                                                               |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

      .. note::

         The CPU limit is supported only by clusters of version 8.1.3 or later.

   b. Click **OK**.

#. Associate exception rules.

   a. .. _en-us_topic_0000001951848321__li1978614455413:

      Associate exception rules.

      |image1|

   b. Unbind exception rules.

      .. note::

         -  Only clusters of 8.2.0 and later versions support associating exception rules. For cluster versions earlier than 8.2.0, see :ref:`7.c <en-us_topic_0000001951848321__li134447718510>`.
         -  The default exception rules take effect for users not associated with any resource pools, and for users whose resource pools do not have any exception rules configured. If a user-defined rule is associated with a resource pool, this rule prevails in the pool.

            -  The default exception rules are supported only by clusters of version 8.2.0 or later. After a cluster of an earlier version is upgraded to version 8.2.0 or later, the default exception rules do not take effect. You can create exception rules as needed.
            -  The cluster version 8.2.1 supports downgradation of exception rules. All exception rules support downgradation behaviors. After downgradation, only network resource preemption is downgraded to a low priority. Downgraded network queries are scheduled only when there is no normal queries.
            -  A resource pool can be associated with up to 16 exception rules.

         -  A resource pool can be associated with multiple groups of exception rules, which work in an OR way. One group of exception rules works if all its conditions are met. For example, a resource pool is associated with two groups of rules. One group specifies **elapsedtime=2400**, and the other group specifies **elapsedtime=1200** and **memsize=2000**. If the execution time of a job reaches 1200 seconds and the memory usage reaches 2000 MB, or if the execution time reaches 2400 seconds, the job will be terminated.

   c. .. _en-us_topic_0000001951848321__li134447718510:

      Modify the exception rules.

   Modify rule parameters. See the following table for more information.

   .. _en-us_topic_0000001951848321__table450693015419:

   .. table:: **Table 2** Exception rule parameters

      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Parameter                           | Description                                                                                                                                                                                                                    | Value Range (0 Means No Limit)                                                  | Operation                                          |
      +=====================================+================================================================================================================================================================================================================================+=================================================================================+====================================================+
      | Blocking Time                       | Job blocking time. It refers to the total time spent in global and local concurrent queuing. The unit is second.                                                                                                               | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated**, **Downgraded**, or **Not limited** |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     | For example, if the blocking time is set to 300s, a job executed by a user in the resource pool will be terminated after being blocked for 300 seconds.                                                                        |                                                                                 |                                                    |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Execution Time                      | Time that has been spent in executing the job, in seconds.                                                                                                                                                                     | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated**, **Downgraded**, or **Not limited** |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     | For example, if **Time required for execution** is set to 100s, a job executed by a user in the resource pool will be terminated after being executed for more than 100 seconds.                                               |                                                                                 |                                                    |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Total CPU time on all DNs.          | Total CPU time spent in executing a job on all DNs, in seconds.                                                                                                                                                                | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated**, **Downgraded**, or **Not limited** |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Interval for Checking CPU Skew Rate | Interval for checking the CPU skew, in seconds. This parameter must be set together with **Total CPU Time on All DNs**.                                                                                                        | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated**, **Downgraded**, or **Not limited** |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Total CPU Time Skew Rate on All DNs | CPU time skew rate of a job executed on DNs. The value depends on the setting of **Interval for Checking CPU Skew Rate**.                                                                                                      | An integer in the range 1 to 100. The value **0** indicates no limit.           | **Terminated**, **Downgraded**, or **Not limited** |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Data Spilled to Disk Per DN         | Allowed maximum job data spilled to disks on a DN. The unit is MB.                                                                                                                                                             | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated**, **Downgraded**, or **Not limited** |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     | .. note::                                                                                                                                                                                                                      |                                                                                 |                                                    |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     |    This rule is supported only by clusters of version 8.2.0 or later.                                                                                                                                                          |                                                                                 |                                                    |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Average CPU Usage Per DN            | Average CPU usage of a job on each DN. If **Interval for Checking CPU Skew Rate** is configured, the interval takes effect for this parameter. If the interval is not configured, the check interval is 30 seconds by default. | An integer in the range 1 to 100. The value **0** indicates no limit.           | **Terminated**, **Downgraded**, or **Not limited** |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     | .. note::                                                                                                                                                                                                                      |                                                                                 |                                                    |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     |    This rule is supported only by clusters of version 8.2.0 or later.                                                                                                                                                          |                                                                                 |                                                    |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+
      | Maximum Bandwidth on a Single DN    | Maximum network bandwidth (MB) for a job on a single DN.                                                                                                                                                                       | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated**, **Downgraded**, or **Not limited** |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     | .. note::                                                                                                                                                                                                                      |                                                                                 |                                                    |
      |                                     |                                                                                                                                                                                                                                |                                                                                 |                                                    |
      |                                     |    This rule is supported only by clusters of version 8.2.1 or later.                                                                                                                                                          |                                                                                 |                                                    |
      +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+----------------------------------------------------+

   .. note::

      Exception rules allow you to control exceptions of jobs executed by users in a resource pool. Currently, you can configure the parameters listed in :ref:`Table 2 <en-us_topic_0000001951848321__table450693015419>`.

      -  If you select **Terminate** or **Downgrade**, you need to set the corresponding time or percentage.
      -  If you select **Not limited**, the corresponding execution rule does not take effect.
      -  Only cluster 8.2.0 and later versions support the function of modifying exception rules.

#. Associate users.

   .. note::

      -  The resources used by a user to run jobs can be controlled only after the user is added to a resource pool.
      -  A database user can be added to only one resource pool. Users removed from a resource pool can be added to another pool.
      -  Database administrators cannot be associated.
      -  If no resource pools are associated with a user, the user will be associated with **default_pool** by default, and its resource usage will be restricted by **default_pool**. The **default_pool** will be automatically created after resource management is enabled.

   a. Click **Add**.
   b. Select the users to be added from the current user list. You can select multiple users at a time.
   c. Click **OK**.
   d. To remove a user, click **Disassociate User** in the **Operation** column of the user.

.. |image1| image:: /_static/images/en-us_image_0000001924569616.png
