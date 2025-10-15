:original_name: dws_04_0404.html

.. _dws_04_0404:

Cluster Performance Analysis
============================

The node specifications of different GaussDB(DWS) clusters may vary in terms of the number of CPU cores, memory capacity, and node storage capacity. Different specifications lead to different service handling capacity and performance. Before creating a cluster, you need to select the appropriate cluster specifications based on the actual workloads and application scenario.

If the workloads increase, more resources (such as CPU, memory, and network bandwidth) will be needed in order to maintain the same level of database performance. Insufficient cluster resources will lead to performance issues.

GaussDB(DWS) provides abundant monitoring metrics that you can use to monitor cluster performance and status, including CPU usage, memory usage, disk usage, disk I/O, and network I/O. For any abnormality, you can check the metrics to locate the root cause.

If your service requires additional compute or storage resources, expand the capacity of an existing cluster by adding more nodes to it or changing node specifications through the management console.
