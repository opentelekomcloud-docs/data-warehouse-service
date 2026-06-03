:original_name: dws_01_07231.html

.. _dws_01_07231:

Creating and Managing a DWS Resource Pool
=========================================

Functions
---------

DWS uses resource pools to separate and control compute resources. It links each database user to a specific resource pool. When a user runs an SQL query, the system directs the query to the corresponding resource pool based on this link. You can specify the number of queries that can be concurrently executed in a resource pool, the upper limit of memory used for a single query, and the memory and CPU resources that can be used by a resource pool. In this way, you can limit and isolate the resources occupied by different workloads, properly utilizing resources to process hybrid database loads and achieve high query performance. After a cluster is converted into a logical cluster, you can create, modify, or delete a resource pool in the logical cluster.

Notes and Constraints
---------------------

-  You can create up to 63 resource pools.
-  **The CPU resource sharing quota is as follows:**

   -  The sum of the parameter values of all the resource pools cannot exceed 99%. If there is only one resource pool, the CPU share parameter does not take effect.

   -  The CPU share parameter takes effect only when CPU contention occurs. For example, resource pools A and B are bound to CPU 1. If A and B are both running, the parameter takes effect. If there is only A running, the parameter does not take effect.

      The sum of the CPU limits of all the resource pools cannot exceed 100%. The default value is 0.

-  **The dedicated CPU resource limits are as follows:**

   -  Only clusters of version 8.1.3 and later versions support the dedicated CPU limit.

   -  You are not advised to allocate multiple dedicated resource pools when the number of CPU cores is small. The minimum value of the dedicated CPU quota is **1**. If the number of CPU cores is small and the resource pool created earlier has used up the remaining CPU cores, the resource pool created later will share the CPU cores of the previous resource pool. As a result, the CPU ratio may be inconsistent with the actual CPU ratio.

      Assume that the cluster has two CPU cores and three resource pools are set up with CPU allocation ratios of 15%, 25%, and 60%. The first core is assigned 1 unit (minimum value), the second core is also assigned 1 unit, and the third core is assigned 1 unit as well, with no additional CPU cores available, sharing the previous CPU.

-  Only clusters of 8.2.1 and later versions support the network bandwidth weight feature.
-  **The constraints for exception rule association are as follows:**

   -  Only clusters of version 8.2.0 or later support association and unbinding of exception rules.
   -  The default exception rule applies when a user has no linked resource pool or when their resource pool lacks specific rules. If a user's resource pool has a defined rule, that rule overrides the default one.

   -  A resource pool can be associated with a maximum of 16 sets of exception rules that operate on an OR basis. Each set activates only when every condition within it is satisfied. A resource pool has two rule sets. The first set ends jobs after 1,200 seconds if they also use 2,000 MB of memory (setting **elapsedtime** to **1200** and **memsize** to **2000**). The second set stops jobs that run for 2,400 seconds, regardless of memory usage (setting **elapsedtime** to **2400**). Jobs terminate when either condition is met.

-  **The constraints for user association are as follows:**

   -  The resources used by a user to run jobs can be controlled only after the user is added to a resource pool.
   -  A database user can be added to only one resource pool. Users removed from a resource pool can be added to another pool.
   -  Database administrators cannot be associated.
   -  If a user does not specify an associated resource pool, the user is associated with **default_pool** by default. The resource usage is restricted by **default_pool**. The **default_pool** will be automatically created after resource management is enabled.

.. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_section1239585215495:

Creating a resource pool
------------------------

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. Click **Add Resource Pool**.

#. Set the parameters listed in :ref:`Table 1 <en-us_topic_0000002344532677__en-us_topic_0000001372520138_en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98>`.

   .. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98:

   .. table:: **Table 1** Resource pool parameters

      +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter                     | Description                                                                                                                                                                        | Default Value         |
      +===============================+====================================================================================================================================================================================+=======================+
      | Name                          | Resource pool name.                                                                                                                                                                | ``-``                 |
      +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | CPU Resource (%)              | -  CPU share: Percentage of CPU time that can be used by users associated with the current resource pool to execute jobs. The value is an integer ranging from 1 to 99.            | ``-``                 |
      |                               | -  CPU limit: Maximum percentage of CPU cores used by a database user in a resource pool. The value is an integer ranging from 0 to 100. **0** indicates no limit.                 |                       |
      +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Memory Resource (%)           | Percentage of the memory that can be used by a resource pool.                                                                                                                      | 0 (not limited)       |
      |                               |                                                                                                                                                                                    |                       |
      |                               | **You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.** |                       |
      +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Storage Resource (MB)         | Size of the permanent tablespace that can be used.                                                                                                                                 | -1 (not limited)      |
      |                               |                                                                                                                                                                                    |                       |
      |                               | **This parameter indicates the total tablespace of all DNs in a resource pool. Available space of a single DN = Configured value/Number of DNs.**                                  |                       |
      +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Complex Statement Concurrency | Maximum number of concurrent queries in a resource pool.                                                                                                                           | 10                    |
      |                               |                                                                                                                                                                                    |                       |
      |                               | **You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.** |                       |
      +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Network Bandwidth Weight      | Weight for network scheduling. The value is an integer ranging from 1 to 2147483647. The default value is **-1**.                                                                  | -1 (not limited)      |
      +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

Modifying a Resource Pool
-------------------------

You can modify the parameters of a resource pool on the resource management page.

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. In the **Resource Pool** drop-down list, select a resource pool. Set the required parameters, including :ref:`Short Query Configuration <en-us_topic_0000002344532677__en-us_topic_0000001372520138_li7697151185212>`, :ref:`Resource Configuration <en-us_topic_0000002344532677__en-us_topic_0000001372520138_li1753123855211>`, :ref:`Associated Exception Rules <en-us_topic_0000002344532677__en-us_topic_0000001372520138_li39761551523>`, and :ref:`User Association <en-us_topic_0000002344532677__en-us_topic_0000001372520138_li964110195312>`. (You can modify them independently.)

#. .. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_li7697151185212:

   **Modify the short query configuration.**

   a. Click **Edit** on the right of **Short Query Configuration** to modify the parameters. For details, see :ref:`Table 2 <en-us_topic_0000002344532677__en-us_topic_0000001372520138_table1217141618468>`.

      .. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_table1217141618468:

      .. table:: **Table 2** Short query configuration parameters

         +------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+---------+
         | Parameter                    | Description                                                                                                                                             | Value   |
         +==============================+=========================================================================================================================================================+=========+
         | Short Query Acceleration     | Whether to enable short query acceleration. This function is enabled by default.                                                                        | Enabled |
         +------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+---------+
         | Simple Statement Concurrency | A short query is a job whose estimated memory used for execution is less than 32 MB. The default value **-1** indicates that the job is not controlled. | 10      |
         +------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+---------+

   b. Ensure that the settings are correct and click **Save**.

#. .. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_li1753123855211:

   **Modify the resource configuration.**

   a. Click **Edit** on the right of **Resource Configuration** to modify the parameters. For details, see :ref:`Table 3 <en-us_topic_0000002344532677__en-us_topic_0000001372520138_table1872071185219>`.

      .. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_table1872071185219:

      .. table:: **Table 3** Resource configuration parameters

         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Parameter                     | Description                                                                                                                                                                        | Default Value         |
         +===============================+====================================================================================================================================================================================+=======================+
         | Name                          | Resource pool name.                                                                                                                                                                | ``-``                 |
         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | CPU Share                     | Percentage of CPU time that can be used by users associated with the current resource pool to execute jobs. The value is an integer ranging from 1 to 99.                          | ``-``                 |
         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | CPU Limit                     | Maximum percentage of CPU cores used by a database user in a resource pool. The value is an integer ranging from 0 to 100. **0** indicates no limit.                               | ``-``                 |
         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Memory Resource (%)           | Percentage of the memory that can be used by a resource pool.                                                                                                                      | 0 (not limited)       |
         |                               |                                                                                                                                                                                    |                       |
         |                               | **You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.** |                       |
         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Storage Resource (MB)         | Size of the permanent tablespace that can be used.                                                                                                                                 | -1 (not limited)      |
         |                               |                                                                                                                                                                                    |                       |
         |                               | **This parameter indicates the total tablespace of all DNs in a resource pool. Available space of a single DN = Configured value/Number of DNs.**                                  |                       |
         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Complex Statement Concurrency | Maximum number of concurrent queries in a resource pool.                                                                                                                           | 10                    |
         |                               |                                                                                                                                                                                    |                       |
         |                               | **You can manage memory and query concurrency separately or jointly. Under joint management, jobs can be delivered only when both the memory and concurrency conditions are met.** |                       |
         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
         | Network Bandwidth Weight      | Weight for network scheduling. The value is an integer ranging from 1 to 2147483647. The default value is **-1**.                                                                  | -1 (not limited)      |
         +-------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   b. Click **OK**.

#. .. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_li39761551523:

   **Associate exception rules.**

   a. Click **Associated Exception Rules** on the left.
   b. Select the exception rules to be associated from the current exception rule list. You can select multiple exception rules at a time.
   c. Click **OK**.
   d. (Optional) To unbind an exception rule, click **Disassociate Rule**.

#. .. _en-us_topic_0000002344532677__en-us_topic_0000001372520138_li964110195312:

   **Associate users.**

   a. Click **User Association** on the left.

   b. In the current user list, select the users to be associated. You can select multiple users at a time.

      In the user binding list, the lock status can be **Unlocked**, **Locked**, or **Unknown**. In versions before 8.5.0.100, only **Unknown** is shown for user lock status. A locked user cannot be chosen for association as the selection button is disabled. An unknown user can be selected, but successful binding depends on the user's actual lock status.

   c. Click **OK**.

   d. (Optional) To disassociate a user, click **Disassociate User**.

Deleting a Resource Pool
------------------------

#. Log in to the DWS console.

#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.

#. In the navigation pane, choose **Resource Management**.

#. In the **Resource Pools** area on the left, click the name of a resource pool.

#. Click **Delete Resource Pool** on the right.

   Deleting a resource pool that is associated with a database user is not allowed. To delete the resource pool, you need to first disassociate it from the database user.

#. Click **OK**.
