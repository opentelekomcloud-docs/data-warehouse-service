:original_name: dws_01_07233.html

.. _dws_01_07233:

Adding Workload Queues
======================

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Click the plus sign (+) next to **Workload Queue**.

   |image1|

   .. note::

      You can create a maximum of 63 workload queues.

#. Enter the name and configure related resources for a new workload queue by referring to :ref:`Table 1 <en-us_topic_0000001134560498__en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98>`.

   .. _en-us_topic_0000001134560498__en-us_topic_0000001076708629_t9f2b382ce2fe42968c6411f0d6ac5d98:

   .. table:: **Table 1** Configuring workload queue parameters

      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                       | Value                 |
      +=======================+===================================================================================================================================================================================+=======================+
      | Name                  | Name of a workload queue.                                                                                                                                                         | queue_test            |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | CPU Usage (%)         | Percentage of the CPU time slice that can be used by database users in a queue for job execution.                                                                                 | 20                    |
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

   |image2|

#. Confirm the information and click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001180320321.png
.. |image2| image:: /_static/images/en-us_image_0000001180440259.png
