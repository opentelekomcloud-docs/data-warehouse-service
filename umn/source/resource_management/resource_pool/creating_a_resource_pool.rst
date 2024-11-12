:original_name: dws_01_07233.html

.. _dws_01_07233:

Creating a Resource Pool
========================

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Click **Add Resource Pool**.

   .. note::

      Up to 63 resource pools can be created.

#. Configure the resource pool. For more information, see :ref:`Table 1 <en-us_topic_0000001924728788__en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98>`.

   .. _en-us_topic_0000001924728788__en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98:

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

#. Confirm the information and click **OK**.
