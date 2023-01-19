:original_name: dws_04_0472.html

.. _dws_04_0472:

.. _en-us_topic_0000001100991256:

Suggestions for SMP Parameter Settings
======================================

Starting from this version, SMP auto adaptation is enabled. For newly deployed clusters, the default value of **query_dop** is **0**, and SMP parameters have been adjusted. To ensure forward compatibility, the value of **query_dop** should remain unchanged after an existing cluster is upgraded.

For an upgraded cluster, if you want to set **query_dop** to **0** and enable SMP parallel processing, modify the following parameters to obtain better dop options:

-  comm_usable_memory

   If the system memory is large, the value of **max_process_memory** is large. In this case, you are advised to set the value of this parameter to 5% of **max_process_memory**, that is, 4 GB by default.

-  comm_max_stream

   The recommended value for this parameter is calculated as follows: comm_max_stream = Min(dop_limit x dop_limit x 20 x 2, max_process_memory (bytes) x 0.025/Number of DNs/260). The value must be within the value range of **comm_max_stream**.

-  max_connections

   The recommended value for this parameter is calculated as follows: max_connections = dop_limit x 20 x 6 + 24. The value must be within the value range of **max_connections**.

   .. caution::

      In the preceding formulas, **dop_limit** indicates the number of CPUs corresponding to each DN in the cluster. It is calculated as follows: **dop_limit** = Number of logical CPU cores of a single server/Number of DNs of a single server.
