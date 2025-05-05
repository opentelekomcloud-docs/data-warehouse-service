:original_name: dws_01_07234.html

.. _dws_01_07234:

Modifying a Resource Pool
=========================

You can modify the parameters of a resource pool on the resource management page.

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. In the **Resource Pools** drop-down list, click the name of a resource pool, including **Short Query Configuration**, **Resource Configuration**, **Associated Exception Rules**, and **Associated User**.

#. Modify the short query configuration. Set the parameters as required and click **Save** on the right.

   +------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+
   | Parameter                    | Description                                                                                                                                             | Value  |
   +==============================+=========================================================================================================================================================+========+
   | Short Query Acceleration     | Whether to enable short query acceleration. This function is enabled by default.                                                                        | Enable |
   +------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+
   | Simple Statement Concurrency | A short query is a job whose estimated memory used for execution is less than 32 MB. The default value **-1** indicates that the job is not controlled. | 10     |
   +------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+--------+

#. Modify the resource configuration.

   a. Click **Edit** on the right and modify the parameters according to :ref:`Table 1 <en-us_topic_0000002203426541__table1380933134011>`.

      .. _en-us_topic_0000002203426541__table1380933134011:

      .. table:: **Table 1** Resource pool parameters

         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Parameter                     | Description                                                                                                                                                                                                                                                  | Default Value         |
         +===============================+==============================================================================================================================================================================================================================================================+=======================+
         | Name                          | Resource pool name.                                                                                                                                                                                                                                          | ``-``                 |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | CPU Share                     | Percentage of CPU time that can be used by users associated with the current resource pool to execute jobs. The value is an integer ranging from 1 to 99.                                                                                                    | ``-``                 |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | .. note::                                                                                                                                                                                                                                                    |                       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               |    -  The sum of the parameter values of all the resource pools cannot exceed 99%. If there is only one resource pool, the CPU share parameter does not take effect.                                                                                         |                       |
         |                               |    -  The CPU share parameter takes effect only when CPU contention occurs. For example, resource pools A and B are bound to CPU 1. If A and B are both running, the parameter takes effect. If there is only A running, the parameter does not take effect. |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | CPU Limit                     | Maximum percentage of CPU cores used by a database user in a resource pool. The value is an integer ranging from 0 to 100. **0** indicates no limit.                                                                                                         | ``-``                 |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | .. note::                                                                                                                                                                                                                                                    |                       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               |    -  The sum of the CPU limits of all the resource pools cannot exceed 100%. The default value is 0.                                                                                                                                                        |                       |
         |                               |    -  Only clusters of 8.1.3 and later versions support the dedicated CPU limit.                                                                                                                                                                             |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Memory Resource (%)           | Percentage of the memory that can be used by a resource pool.                                                                                                                                                                                                | 0 (not limited)       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.                                                                               |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Storage Resource (MB)         | Size of the permanent tablespace that can be used.                                                                                                                                                                                                           | -1 (not limited)      |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | This parameter indicates the total tablespace of all DNs in a resource pool. Available space of a single DN = Configured value/Number of DNs.                                                                                                                |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Complex Statement Concurrency | Maximum number of concurrent queries in a resource pool.                                                                                                                                                                                                     | 10                    |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.                                                                               |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Network Bandwidth Weight      | Weight for network scheduling. The value is an integer ranging from 1 to 2147483647. The default value is **-1**.                                                                                                                                            | -1 (not limited)      |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               | .. caution::                                                                                                                                                                                                                                                 |                       |
         |                               |                                                                                                                                                                                                                                                              |                       |
         |                               |    CAUTION:                                                                                                                                                                                                                                                  |                       |
         |                               |    Only clusters of 8.2.1 and later versions support the network bandwidth weight feature. Unfortunately, clusters do not support this.                                                                                                                      |                       |
         +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

      .. note::

         The CPU limit is supported only by clusters of version 8.1.3 or later.

   b. Click **OK**.

#. .. _en-us_topic_0000002203426541__en-us_topic_0000001076579461_en-us_topic_0254317345_li1213141321:

   Associate exception rules.

   a. Click **Associated Exception Rules** on the left.
   b. Select the exception rules to be associated from the current exception rule list. You can select multiple exception rules at a time.
   c. Click **OK**.
   d. To unbind an exception rule, click **Disassociate Rule**.

      .. note::

         -  Only clusters of version 8.2.0 or later support association and unbinding of exception rules.
         -  The default exception rules take effect for users not associated with any resource pools, and for users whose resource pools do not have any exception rules configured. If a user-defined rule is associated with a resource pool, this rule prevails in the pool.

            -  The default exception rules are supported only by clusters of version 8.2.0 or later. After a cluster of an earlier version is upgraded to version 8.2.0 or later, the default exception rules do not take effect. You can create exception rules as needed.
            -  The cluster version 8.2.1 supports downgradation of exception rules. All exception rules support downgradation behaviors. After downgradation, only network resource preemption is downgraded to a low priority. Downgraded network queries are scheduled only when there is no normal queries.
            -  A resource pool can be associated with up to 16 exception rules.

         -  A resource pool can be associated with multiple groups of exception rules, which work in an OR way. One group of exception rules works if all its conditions are met. For example, a resource pool is associated with two groups of rules. One group specifies **elapsedtime=2400**, and the other group specifies **elapsedtime=1200** and **memsize=2000**. If the execution time of a job reaches 1,200 seconds and the memory usage reaches 2,000 MB, or if the execution time reaches 2,400 seconds, the job will be terminated.

#. Associate users.

   a. Click **User Association** on the left.
   b. In the current user list, select the users to be associated. You can select multiple users at a time.
   c. Click **OK**.
   d. To disassociate a user, click **Disassociate User**.

      .. note::

         -  The resources used by a user to run jobs can be controlled only after the user is added to a resource pool.
         -  A database user can be added to only one resource pool. Users removed from a resource pool can be added to another pool.
         -  In the user binding list, the lock status can be unlocked, locked, or unknown. In versions before 8.5.0.100, only "Unknown" is shown for user lock status. A locked user cannot be chosen for association as the selection button is disabled. An unknown user can be selected, but successful binding depends on the user's actual lock status.
         -  Database administrators cannot be associated.
         -  If no resource pools are associated with a user, the user will be associated with **default_pool** by default, and its resource usage will be restricted by **default_pool**. The **default_pool** will be automatically created after resource management is enabled.
