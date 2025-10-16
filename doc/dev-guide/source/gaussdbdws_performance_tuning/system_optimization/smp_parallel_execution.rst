:original_name: dws_04_0420.html

.. _dws_04_0420:

.. _en-us_topic_0000002088734269:

SMP Parallel Execution
======================

Complex queries may take a long time. In a system with low concurrency support, this can be a problem. SMP is used to implement operator-level parallel execution, which can effectively speed up queries, improving query performance and resource utilization.

The SMP feature improves performance through operator parallelism but may drive more resource usage, including CPU, memory, network, and I/O. In essence, SMP is a method that trades resources for time, meaning it accelerates queries at the cost of additional resources. It improves system performance in appropriate scenarios and when resources are sufficient, but may also deteriorate performance if used inappropriately. Furthermore, compared with serial processing, SMP generates more candidate plans, which is more time-consuming and may hurt performance.

The SMP feature of GaussDB(DWS) is controlled by the GUC parameter **query_dop**. Users use this parameter to specify an appropriate degree of query parallelism.

.. _en-us_topic_0000002088734269__en-us_topic_0000001188642080_s54e19b618557485f8e756dfdd231861e:

Application Scenarios and Constraints for SMP
---------------------------------------------

**Applicable Scenarios**

-  Operators supporting parallel processing are used.

   The execution plan contains the following operators:

   #. Scan: Row Storage common table and a line memory partition table sequential scanning, column-oriented storage ordinary table and column-oriented storage partition table sequential scanning, HDFS internal and external table sequence scanning. Surface scanning GDS data can be imported at the same time. All of the above does not support replication tables.
   #. Join: HashJoin, NestLoop
   #. Agg: HashAgg, SortAgg, PlainAgg, and WindowAgg, which supports only **partition by**, and does not support **order by**.
   #. Stream: Redistribute, Broadcast
   #. Other: Result, Subqueryscan, Unique, Material, Setop, Append, VectoRow, RowToVec

-  SMP-unique operators are used.

   To execute queries in parallel, Stream operators are added for data exchange for the SMP feature. These new operators can be considered as the subtypes of Stream operators.

   #. Local Gather aggregates data of parallel threads within a DN
   #. Local Redistribute redistributes data based on the distributed key across threads within a DN
   #. Local Broadcast broadcasts data to each thread within a DN.
   #. Local RoundRobin distributes data in polling mode across threads within a DN.
   #. Split Redistribute redistributes data across parallel threads on different DNs.
   #. Split Broadcast broadcasts data to all parallel DN threads in the cluster.

   Among these operators, Local operators exchange data between parallel threads within a DN, and non-Local operators exchange data across DNs.

-  Example

   The TPCH Q1 parallel plan is used as an example.

   |image1|

   In this plan, implement the Hdfs Scan and HashAgg operator parallel, and adds the Local Gather and Split Redistribute data exchange operator.

   In this example, the sixth operator is Split Redistribute, and **dop: 4/4** next to the operator indicates that the degree of parallelism of the sender and receiver is 4. 4 No operator is Local Gather, marked dop: 1/4 above, this operator sender thread parallel degree is 4, while the receiving end thread parallelism degree to 1, that is, lower-layer 5 number Hash Aggregate operators according to the 4 parallel degree, while the working mode of the port on the upper-layer 1 to 3 number operator according to the executed one by one, 4 number operator is used to achieve intra-DN concurrent threads data aggregation.

   You can view the parallelism situation of each operator in the dop information.

**Non-Applicable Scenarios**

#. Small queries are performed, where plan generation may account for a significant portion of the total query time.
#. Operators are processed on CNs.
#. Statements that cannot be pushed down are executed.
#. The **subplan** of a query and operators containing a subquery are executed.

Impact of Resource Availability on SMP Performance
--------------------------------------------------

The SMP architecture accelerates queries at the cost of additional resources. After the plan parallelism is executed, more resources are consumed, including the CPU, memory, I/O, and network bandwidth. As the DOP grows, the resource consumption also increases. If these resources become a bottleneck, SMP cannot improve performance. On the contrary, it may do exactly the opposite. Adaptive SMP is provided to dynamically select the optimal parallel degree for each query based on the resource usage and query requirements. The following information describes the situations that the SMP affects theses resources:

-  **CPU resources**

   In a general customer scenario, the system CPU usage rate is not high. Using the SMP parallelism architecture will fully use the CPU resource to improve the system performance. If the number of CPU kernels of the database server is too small and the CPU usage is already high, enabling the SMP parallelism may deteriorate the system performance due to resource compete between multiple threads.

-  **Memory resources**

   The query parallel causes memory usage growth, but the memory upper limit used by each operator is still restricted by **work_mem**. Assume that **work_mem** is 4 GB, and the degree of parallelism is 2, then the memory upper limit of each concurrent thread is 2 GB. When **work_mem** is small or the system memory is sufficient, running SMP parallelism may push data down to disks. As a result, the query performance deteriorates.

-  **Network bandwidth resources**

   To execute queries in parallel, data exchange operators are added. Local stream operators exchange data between threads within a DN. Data is exchanged in memory, so it does not impact network performance. Non-local operators exchange data over the network and increase the network load. If the capacity of a network resource has already become a bottleneck, parallelism may hurt performance.

-  **I/O resources**

   A parallel scan increases I/O resource consumption. It can improve performance only when I/O resources are sufficient.

Other Factors Impacting SMP Performance
---------------------------------------

Besides the resource factor, other factors may also impact SMP performance, such as uneven data distribution across tables and the degree of system parallelism.

-  **Impact of data skew on SMP performance**

   Serious data skew deteriorates parallel execution performance. For example, if the data volume of a value in the join column is much more than that of other values, the data volume of a parallel thread will be much more than that of others after Hash-based data redistribution, resulting in the long-tail issue and poor parallelism performance.

-  **Impact of system parallelism degree on SMP performance**

   The SMP feature uses more resources, and unused resources are decreasing in a high concurrency scenario. Therefore, enabling the SMP parallelism will result in serious resource compete among queries. Once resource competes occur, no matter the CPU, I/O, memory, or network resources, all of them will result in entire performance deterioration. In the high concurrency scenario, enabling the SMP will not improve the performance effect and even may cause performance deterioration.

Suggestions for SMP Parameter Settings
--------------------------------------

To enable the SMP adaptation function, set **query_dop** to **0** and adjust the following parameters to obtain an optimal DOP selection:

-  comm_usable_memory

   If the system memory is large, the value of **max_process_memory** is large. In this case, you are advised to set the value of this parameter to 5% of **max_process_memory**, that is, 4 GB by default.

-  comm_max_stream

   The recommended value for this parameter is calculated as follows: comm_max_stream = Min(dop_limit x dop_limit x 20 x 2, max_process_memory (bytes) x 0.025/Number of DNs/260). The value must be within the value range of **comm_max_stream**.

-  max_connections

   The recommended value for this parameter is calculated as follows: max_connections = dop_limit x 20 x 6 + 24. The value must be within the value range of **max_connections**.

   .. caution::

      In the preceding formulas, **dop_limit** indicates the number of CPUs corresponding to each DN in the cluster. It is calculated as follows: **dop_limit** = Number of logical CPU cores of a single server/Number of DNs of a single server.

SMP Configuration Procedure
---------------------------

.. important::

   The CPU, memory, I/O, and network bandwidth resources are sufficient. In essence, SMP is a method that trades resources for time. After the plan parallelism is executed, resource consumption increases. When these resources become a bottleneck, SMP may deteriorate, rather than improve performance. In addition, it takes a longer time to generate SMP plans than serial plans. Therefore, in TP services that mainly involve short queries or in case resources are insufficient, you are advised to disable SMP by setting **query_dop** to **1**.

**Procedure**:

#. Observe the current system load situation. If the resource is sufficient (the resource usage ratio is smaller than 50%), perform step 2. Otherwise, exit this system.

#. Set **query_dop** to **1** (default value). Use **explain** to generate an execution plan and check whether the plan can be used in scenarios described in :ref:`Application Scenarios and Constraints for SMP <en-us_topic_0000002088734269__en-us_topic_0000001188642080_s54e19b618557485f8e756dfdd231861e>`. If the plan can be used, go to the next step.

#. Set **query_dop=-**\ *value*. The value range of the parallelism degree is [1, *value*].

#. Set **query_dop=**\ *value*. The parallelism degree is 1 or *value*.

#. Before the query statement is executed, set **query_dop** to an appropriate value. After the statement is executed, set **query_dop** to **off**. For example:

   ::

      SET query_dop = 0;
      SELECT COUNT(*) FROM t1 GROUP BY a;
      ......
      SET query_dop = 1;

   .. note::

      -  If resources are sufficient, the higher the degree of parallelism, the better the performance improvement result.
      -  The SMP parallelism degree supports a session level setting and you are advised to enable SMP before executing queries that meet the requirements. After the execution is complete, disable SMP. Otherwise, SMP may affect services during peak hours.
      -  SMP adaptability (**query_dop** <= 0) depends on resource management. If resource management is disabled, only plans with parallelism degree of only 1 or 2 will be generated.

.. |image1| image:: /_static/images/en-us_image_0000002079863805.jpg
