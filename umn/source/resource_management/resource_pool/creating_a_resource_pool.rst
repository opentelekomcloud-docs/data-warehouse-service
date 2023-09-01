:original_name: dws_01_07233.html

.. _dws_01_07233:

Creating a Resource Pool
========================

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Click **Add Resource Pool**.

   |image1|

   .. note::

      Up to 63 resource pools can be created.

#. Configure the resource pool. For more information, see :ref:`Resource pool parameters <en-us_topic_0000001466594906__en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98>`.

   |image2|

   .. _en-us_topic_0000001466594906__en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98:

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

#. Confirm the information and click **OK**.

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000001517355561.png
.. |image2| image:: /_static/images/en-us_image_0000001517754585.png
.. |image3| image:: /_static/images/en-us_image_0000001467074382.png
